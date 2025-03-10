# static-libgit2

This repository makes it easier to include the C library [Libgit2](https://libgit2.org) into an iOS or Mac application. It does *not* try to provide any sort of nice, Swifty wrapper over the `libgit2` APIs.

This repository is heavily indebted to https://github.com/light-tech/LibGit2-On-iOS. However, the `LibGit2-On-iOS` project doesn't expose the C Language bindings as its own Swift Package, choosing instead to use their framework as a binary target in their Swift Language binding project [MiniGit](https://github.com/light-tech/MiniGit). If you want Swift bindings, you should probably use that project! However, if you want to work directly with the C API, _this_ is the project for you want to start with.

## Usage in an Application

If you are writing an iOS or Mac app that needs access to `libgit2`, you can simply add this package to your project via Swift Package Manager. The `libgit2` C Language APIs are provided through the `Clibgit2` module, so you can access them with `import Clibgit2`. For example, the following SwiftUI view will show the `libgit2` version:

```swift
import Clibgit2
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text(LIBGIT2_VERSION)
            .padding()
    }
}
```

## Usage in another package

If you want to use `static-libgit2` in another package (say, to expose some cool Swift bindings to the C API), include the following in your `Package.swift`:

```swift
    dependencies: [
      .package(url: "https://github.com/bdewey/static-libgit2", from: "0.1.0"),
    ],
```

# What's Included

`static-libgit2` includes the following libraries:

| Library | Version |
| ------- | ------- |
| libgit2 | 1.9.0   | https://github.com/libgit2/libgit2/	
| openssl | 3.4.0   | https://github.com/openssl/openssl/
| libssh2 | 1.11.1  | https://github.com/libssh2/libssh2/

Adjust build-libgit2-framework.sh to use different versions of those libs, find most current versions on GitHub using the links above.

Caution: OpenSSL 3.1.0 to 3.1.x does not load on an apple silicon, because of processor probing. (I tried to use 3.1.1) 
See discussion: https://github.com/openssl/openssl/issues/20155
OpenSSL 3.0.9 was used before 3.2.0

This build recipe and the original version of the build script comes from the insightful project https://github.com/light-tech/LibGit2-On-iOS. 

# Build it yourself

You don't need to depend on this package's pre-built libraries. You can build your own version of the framework.

```
# You need the tool `wget` and the tool `cmake`
brew install wget
brew install cmake

# Clone the static-libgit2 repo
git clone https://github.com/bdewey/static-libgit2

# Run the build script
cd static-libgit2
./build-libgit2-framework.sh
```
