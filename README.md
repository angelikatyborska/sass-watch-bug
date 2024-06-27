# Sass watch bug

Switching branches while `sass --watch` is running causes a `Error: Can't find stylesheet to import.` error when the switch causes one of the dependant files to be removed even when the file is no longer needed after the branch switch.

Example: In this repository, on the `apple` branch, we `@use` the `src/components/_apple.scss` file in `in.scss`:

```scss
@use "./src/components/apple";
```

On the `main` branch, we do **not** `@use` the `_apple.scss` file and the file does **not** exist.

If I start sass in watch mode while on the `apple` branch and then checkout the `main` branch, the build errors with this error:

```
/* Error: Can't find stylesheet to import.
 *   ,
 * 3 | @use "./src/components/apple";
 *   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
 *   '
 *   in.scss 3:1  root stylesheet */

```

This error doesn't make sense because the underlined line should no longer exist after checking out the `main` branch. If you stop the sass process and start it again, or if you make any other change to the watched files, the next build will succeed.

Please see the [screen recording](./screen-recording.mov) that demonstrates this bug.
