title: Semantic Versioning and Dependencies
date: 2011/03/10
author: Brian J Brennan
tags: development node npm versioning

Dependencies management is hell. Inconsistent versioning contributes to the problem; this is why [mojombo](https://github.com/mojombo) formalized [Semantic Versioning](http://semver.org/). It gives meaning to the MAJOR.MINOR.PATCH version system that many (most?) software projects use. Instead of bumping versions because something *feels* like a minor release, or *maybe* it's big enough to be a major release, semver prescribes a set of rules for when to do what.

If you haven't yet read the semver spec, click that link I gave you a few sentence ago and read the whole thing. I encourage you to implement it if you provide any sort of public API.

[npm](http://npmjs.org/) enforces semver for its packages. By that I mean it enforces the versioning number scheme, and tells users to read the semver spec when they run `npm help json`.

I find, however, that a lot of developers define their dependencies without thinking about what the versions mean. Out in the wild I see a lot of package.json files, even in well used libraries, with dependency sections that look like this:

    "dependencies": {
      "colors": ">= 0.3.0",
      "eyes": ">= 0.1.6",
      "loggly": ">= 0.1.4",
      "riak-js": ">= 0.3.0beta4",
      "vows": ">= 0.5.2"
    }

Increases in the major version number of a package are a strong indicator that some backwards-incompatible changes have occurred in the API. If a package defines a minimum version for it's dependencies, it should almost always define an upper bound to be less than the next major version.

    "dependencies": {
      "colors": ">= 0.3.0 < 1.0.0",
      "eyes": ">= 0.1.6 < 1.0.0",
      "loggly": ">= 0.1.4 < 1.0.0",
      "riak-js": ">= 0.3.0beta4 < 1.0.0",
      "vows": ">= 0.5.2 < 1.0.0"
    }

I've been burned on this before – I `npm install`d a package, but between when the package was released and when I installed it, one of its dependencies went from 0.x.y to 1.0.0. `npm` merrily installed the latest version, which was totally incompatible with the version of the package I installed.

Now, to rain on my own parade, I need to point out two sections of the semver spec:

> Major version zero (0.y.z) is for initial development. Anything may change at any time. The public API should not be considered stable.

and

> Version 1.0.0 defines the public API. The way in which the version number is incremented is now dependent on this public API and how it changes.

So in the case of the dependencies I mentioned above, technically anything could change between, say, vows@0.5.2 and vows@0.5.3. It's unlikely, but can't be ruled out. While in <1.0.0, it's not the library author's responsibility to maintain any sort of compatibility because pre 1.0.0 the public API doesn't have to be defined. I think the general point stands, though, that dependencies with no upper bounds, especially in libraries that are widely used, is dangerous.
