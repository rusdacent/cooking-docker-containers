# Hadolint demo

## Install Hadolint

https://github.com/hadolint/hadolint?tab=readme-ov-file#install

## Example

Запустим проверку

```
$ hadolint Dockerfile
```

или

```
$ docker run --rm -i hadolint/hadolint < hadolint/Dockerfile.error
```

Результаты
```
-:3 DL3020 error: Use COPY instead of ADD for files and folders

-:4 DL3003 warning: Use WORKDIR to switch to a directory

-:5 DL3007 warning: Using latest is prone to errors if the image will ever update. Pin the version explicitly to a release tag

-:7 DL3059 info: Multiple consecutive `RUN` instructions. Consider consolidation.
```

Проверяем версию с исправлениями

```
$ docker run --rm -i hadolint/hadolint < hadolint/Dockerfile.fix
```

via: [How to use a Dockerfile linter](https://bell-sw.com/blog/how-to-use-a-dockerfile-linter/)