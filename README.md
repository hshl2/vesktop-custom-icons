# vesktop-custom-icons-git

This is a simple clone of the [vesktop-git](https://aur.archlinux.org/packages/vesktop-git) package in the Arch User Repository that injects the original Discord icons into the build process. This doesn’t touch Vesktop at all. It simply generates and renames the source icons provided here into the same names as the files used for Vesktop and injects them into the build process, thereby baking them into the application. No custom CSS or desktop files are needed, and it will survive the built-in update process.

`colors.conf` has a predefined list of common colors. By default, the application will build with the official Discord color palette.

Added dependencies are `imagemagick` and `optipng` for manipulating the source svg icon.

## Build Process

```
paru -S vesktop-custom-icons-git
```

Clone and set your own color - or the whole icon. 
```
color=red makepkg -si
```
