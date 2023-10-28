# DEB-file-creation
A custom tutorial for creating a .deb file in Linux OS.

## Preparations
Prepare the script, file, or anything else that you want to package into a .deb file.

For this tutorial, we will use the "print-my-song" script.

## Create a Package Directory
```
mkdir -p my-package/debian
```

## Collect Dependencies
Use the objdump tool to check dependencies:
```
objdump -p say-my-name
```

Look for the Dynamic Section:
```
Dynamic Section:
  NEEDED               libdl.so.2
  NEEDED               libz.so.1
  NEEDED               libpthread.so.0
  NEEDED               libc.so.6
  INIT                 0x000000000040200
```
Make note of the libraries with the "NEEDED" tag.

### Locate Libraries
To find out which packages provide these libraries, use the dpkg command for each library:
dpkg -S [your-lib-name]
You'll need this information later.

## Create a "control" file
```
nano my-package/debian/control
```

Add the following contents to the file:
```
Package: my-package
Version: 1.0
Section: unknown
Priority: optional
Depends: [your-lib-dependency-1], [your-lib-dependency-2]
Architecture: amd64
Essential: no
Installed-Size: 20
Maintainer: name.com <name@email.com>
Description: Prints song text
Create a Directory for Your Script
mkdir -p my-package/usr/bin
```

### Move your script into this directory:
```
mv ./print-my-song my-package/usr/bin
```

## Build the Package
Finally, build your package:
```
dpkg-deb --build ./my-package
```

You can now install it using apt:
```
apt install ./my-package.deb
```

Once installed, executing the script will display the song's text. Congratulations, you're done!
