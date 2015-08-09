kn-release
==========

Build, tag, push, and publish your repositories with one command.

Based on the lovely https://github.com/rpflorence/rf-release minus the changelog generation. I prefer managing the changelog manually, plus it's very easy to make that an additional step on per project.

Installation
------------

    $ npm install --save kn-release

Usage
-----

Add it to your `package.json` scripts:

```
"scripts": {
  "release": "release --build"
}
```

And now run
  
    $ npm run release

to release new versions of your package.

Build step
----------

Pass `--build` flag to `release` to run `npm run build` after versioning, but before commiting, pushing and publishing. In this case, the `build` directory gets published to npm, not the root, so make sure to copy `package.json` and `README.md` to your build dir in this case.

Here's an example build.sh I use for some of my projects:

```sh
rm -rf build

# npm CJS
babel -d build/lib ./lib
cp README.md CHANGELOG.md index.js package.json build

# standalone UMD
webpack index.js build/standalone.js

# print out the size info
echo "\nnpm build including deps is\n `bro-size build/index.js`"
echo "\nnpm build excluding deps is\n `bro-size build/index.js -u react`"
```

What it does
------------

1. prompts you for the new version
2. runs tests
3. updates `package.json`
4. optionally runs `npm run build` and publishes the `build` dir instead of the root
5. commits the version updates with a simple commit message ("Release {version}")
6. creates the tag "v{version}"
7. pushes master and the new tag to origin
8. publishes to npm

But I have another step before I can tag and publish!
-----------------------------------------------------

That's fine, just do your build first w/o committing, then run
`release`. It'll all go in the same commit.
