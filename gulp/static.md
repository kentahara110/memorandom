# gulpを使った静的ページのセットアップ

## バージョン
node v20

gulp5

## プラグイン
```json
"browser-sync": "^3.0.3",
"gulp": "^5.0.0",
"gulp-autoprefixer": "7",
"gulp-changed": "^5.0.2",
"gulp-clean-css": "^4.3.0",
"gulp-dart-sass": "^1.1.0",
"gulp-group-css-media-queries": "^1.2.2",
"gulp-imagemin": "^9.1.0",
"gulp-newer": "^1.4.0",
"gulp-notify": "^5.0.0",
"gulp-plumber": "^1.2.1",
"gulp-rename": "^2.0.0",
"gulp-sass-glob-use-forward": "^0.1.3",
"gulp-sourcemaps": "^3.0.0",
"gulp-uglify": "^3.0.2",
"gulp-webp": "^5.0.0",
"imagemin-gifsicle": "^7.0.0",
"imagemin-mozjpeg": "^10.0.0",
"imagemin-optipng": "^8.0.0",
"imagemin-pngquant": "^10.0.0",
"imagemin-svgo": "^11.0.1"
```

## ディレクトリ構造
```
dist/assets/css/
            img/
            js/
    index.html
src/assets/scss/
            img/
            js/
    index.html
gulpfile.mjs
```

## gulpfile
拡張子は`mjs`

package.jsonに`"type": "module"`を記述

```mjs
// gulpfile.mjs
import gulp from 'gulp';
import { src, dest, watch, series, parallel } from 'gulp';
import changed from 'gulp-changed';
import imagemin from 'gulp-imagemin';
import imageminSvgo from 'imagemin-svgo';
import imageminOptipng from 'imagemin-optipng';
import imageminGifsicle from 'imagemin-gifsicle';
import mozjpeg from 'imagemin-mozjpeg';
import pngquant from 'imagemin-pngquant';
import gulpDartSass from 'gulp-dart-sass';
import sassGlob from 'gulp-sass-glob-use-forward';
import autoprefixer from 'gulp-autoprefixer';
import cleanCss from 'gulp-clean-css';
import rename from 'gulp-rename';
import uglify from 'gulp-uglify';
import plumber from 'gulp-plumber';
import gcmq from 'gulp-group-css-media-queries';
import sourcemaps from 'gulp-sourcemaps';
import browserSync from 'browser-sync';

// 画像圧縮
function imageMini() {
  return src(['./src/assets/img/**', '!./src/assets/img/**/*.webp'],{encoding: false}) // gulp5ではエンコードをfalseにしないと画像が壊れる
    .pipe(changed('./dist/assets/img'))
    .pipe(
      imagemin([
        pngquant({ // png変換
          quality: [0.65, 0.7],
          speed: 1,
        }),
        mozjpeg({ progressive: true, quality: 75 }), // jpeg変換
        imageminSvgo({
          plugins: [{ removeViewBox: false }],
        }),
        imageminOptipng(),
        imageminGifsicle({ optimizationLevel: 3 }), // gif変換
      ])
    )
    .pipe(dest('./dist/assets/img'));
}

// sass
const sass = () => {
  return gulp
    .src('src/assets/sass/**/*.scss')
    .pipe(plumber({
      errorHandler: function (err) {
        console.log(err.messageFormatted);
        this.emit('end');
      }
    }))
    .pipe(sourcemaps.init())
    .pipe(sassGlob())
    .pipe(gulpDartSass({
      includePaths: ['src/assets/sass'],
      outputStyle:'expanded',
    }))
    .pipe(autoprefixer({cascade: false,}))
    .pipe(gcmq())
    .pipe(rename({extname: '.min.css'}))
    .pipe(cleanCss())
    .pipe(sourcemaps.write('./'))
    .pipe(gulp.dest('dist/assets/css'))
    .pipe(browserSync.stream());
}

// JavaScriptの圧縮
function jsMinify() {
  return src('./src/assets/js/**/*.js')
    .pipe(uglify())
    .pipe(dest('./dist/assets/js'));
}

// htmlをdistへ出力
import newer from 'gulp-newer';
const html = () => {
  return gulp.src('./src/**/*.html')
    .pipe(newer('./dist')) 
    .pipe(gulp.dest('./dist'));
}

// browser-sync
const browserSyncFunc = (done) => {
  browserSync.init({
    server: {
      baseDir: './dist'
    },
    startPath: 'index.html',
    notify: false, // 右上の通知をオフ
  });
}

// リロード
const browserSyncReload = (done) => {
  browserSync.reload();
  done();
}

// 各ファイルを監視
const watchFiles = () => {
  gulp.watch('./src/assets/sass/**/*.scss', sass);
  gulp.watch('./src/**/*.html', gulp.series(html, browserSyncReload));
  gulp.watch('./src/assets/img/**', gulp.series(imageMini, browserSyncReload));
  gulp.watch('./src/assets/js/**.js', gulp.series(jsMinify, browserSyncReload));
}

// タスクの実行
export { imageMini, sass, jsMinify };

// default
export default gulp.series(
  gulp.parallel(html, sass, imageMini),
  jsMinify,
  gulp.parallel(browserSyncFunc, watchFiles)
);
```