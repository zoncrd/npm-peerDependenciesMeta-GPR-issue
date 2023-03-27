# npm-peerDependenciesMeta-GPR-issue

When attempting to install a package that specifies optional `peerDependencies` in the package.json via the `peerDependenciesMeta` attribute from the GitHub Package Registry, optional dependencies are installed anyway.

The problem seems to be caused by a missing property in the [abbreviated package metadata response](https://github.com/npm/registry/blob/master/docs/responses/package-metadata.md#abbreviated-metadata-format). The missing property is the `peerDependenciesMeta` field, without which, npm version 7 and above automatically installs `peerDependencies`.

## Steps to reproduce

- create a new project directory
- create a new npm project: `npm init -y`
- add the GitHub Package Registry as a registry: `npm config set @zoncrd:registry https://npm.pkg.github.com`
- install the package provided by this example repository:
    `npm install @zoncrd/gpr-optional-peer-issue`
- check the installed dependencies: `npm ls npm --depth`

## Expected result
- the `winston` package provided as optional peer dependency should not be installed.

## Actual result
- the `winston` package is installed.
```
check@1.0.0 /Users/zoncrd/Documents/git/oss/npm-peerDependenciesMeta-GPR-issue/check
└─┬ @zoncrd/gpr-optional-peer-issue@0.1.0
  └── winston@3.8.2
```

## Additional information

You can check with the following curl request that the `peerDependenciesMeta` field is missing from the abbreviated package metadata response:
```
curl --location 'https://npm.pkg.github.com/@zoncrd/gpr-optional-peer-issue' \
--header 'Accept: application/vnd.npm.install-v1+json' \
--header 'Authorization: Bearer <GPA>'
```

Response:
```
{
    "_id": "",
    "name": "@zoncrd/gpr-optional-peer-issue",
    "dist-tags": {
        "latest": "0.1.0"
    },
    "versions": {
        "0.1.0": {
            "name": "@zoncrd/gpr-optional-peer-issue",
            "version": "0.1.0",
            "_id": "",
            "_nodeVersion": "16.19.0",
            "_npmVersion": "9.2.0",
            "dist": {
                "integrity": "sha512-CC17RbFH7VXbueM8CPPQHqBCH12n7scU6Y8Yid4F5aZ+brz8fH2bE0ArsuwZ6CKJr1cygWuDh0y684UkvdRDLw==",
                "shasum": "2f516fc8e0ba1888f45a69dba2ac9e241c744950",
                "tarball": "https://npm.pkg.github.com/download/@zoncrd/gpr-optional-peer-issue/0.1.0/2f516fc8e0ba1888f45a69dba2ac9e241c744950"
            },
            "_npmUser": {
                "name": "zoncrd"
            },
            "license": "UNLICENSED",
            "description": "An example package with optional peer dependencies",
            "main": "index.js",
            "scripts": {
                "test": "echo \"Error: no test specified\" && exit 1"
            },
            "author": {
                "name": "zoncrd@gmail.com"
            },
            "peerDependencies": {
                "winston": "^3.8.2"
            },
            "gitHead": "aed7563f31d81435023f7ec665f158d0edb6626e",
            "directories": {},
            "repository": {
                "type": "git",
                "url": "git+https://github.com/zoncrd/npm-peerDependenciesMeta-GPR-issue.git"
            },
            "homepage": "https://github.com/zoncrd/npm-peerDependenciesMeta-GPR-issue#readme",
            "bugs": {
                "url": "https://github.com/zoncrd/npm-peerDependenciesMeta-GPR-issue/issues"
            },
            "_hasShrinkwrap": false,
            "bin": null,
            "readme": "# npm-peerDependenciesMeta-GPR-issue\n\nWhen attempting to install a package that specifies optional `peerDependencies` in the package.json via the `peerDependenciesMeta` attribute from the GitHub Package Registry, optional dependencies are installed anyway.\n\nThe problem seems to be caused by a missing property in the [abbreviated package metadata response](https://github.com/npm/registry/blob/master/docs/responses/package-metadata.md#abbreviated-metadata-format). The missing property is the `peerDependenciesMeta` field, without which, npm version 7 and above automatically installs `peerDependencies`.\n\n## Steps to reproduce\n\n- create a new project directory\n- create a new npm project: `npm init -y`\n- add the GitHub Package Registry as a registry: `npm config set @zoncrd:registry https://npm.pkg.github.com`\n- install the package provided by this example repository:\n    `npm install @zoncrd/gpr-optional-peer-issue`\n- check the installed dependencies: `npm ls`\n\n## Expected result\n- the `winston` package provided as optional peer dependency should not be installed.\n\n## Actual result\n- the `winston` package is installed.\n\n## Additional information\n\nYou can check with the following curl request that the `peerDependenciesMeta` field is missing from the abbreviated package metadata response:\n```\ncurl --location 'https://npm.pkg.github.com/@zoncrd/gpr-optional-peer-issue' \\\n--header 'Accept: application/vnd.npm.install-v1+json' \\\n--header 'Authorization: Bearer <GPA>'\n```\n\nResponse:\n```\n\n```"
        }
    },
    "time": {
        "0.1.0": "2023-03-27T10:29:34Z"
    },
    "description": "An example package with optional peer dependencies",
    "author": {
        "name": "zoncrd@gmail.com"
    },
    "license": "UNLICENSED",
    "homepage": "https://github.com/zoncrd/npm-peerDependenciesMeta-GPR-issue#readme",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/zoncrd/npm-peerDependenciesMeta-GPR-issue.git"
    },
    "bugs": {
        "url": "https://github.com/zoncrd/npm-peerDependenciesMeta-GPR-issue/issues"
    }
}
```