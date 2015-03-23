Cloudbreak Deployer helps to deploy a cloudbreak environment into docker containers.

## Requirements

Right now **Linux** and **OSX** 64 bit binaries are released from Cloudbreak Deployer. For anything else we will create a special docker container.
The deployment itself needs only **Dcoker 1.5.0** or later.

## Installation

To install Cloudbreak Deployer, you just have to unzip the platform specific
single binary to your PATH. The one-liner way is:

```
curl https://raw.githubusercontent.com/sequenceiq/cloudbreak-deployer/master/install | bash
```

## Configuration

Configuration is based on environment variables. Cloudbreak Deployer always forks a new
bash subprocess **without inheriting env vars**. The only way to set env vars relevant to 
Cloudbreak Deployer is to set them in a file called `Profile`.

Actually 2 env vars _are_ inherited: `DEBUG` and `TRACE`

### Env specific Profile

`Profile` is always sourced. For example If you have env specific configurations: prod you need
2 steps:

- create a file called `Profile.prod`
- set the `CBD_DEFAULT_PROFILE` env variable.

To use a specific profile once:
```
CBD_DEFAULT_PROFILE=prod cbd some_commands
```

For permanent setting you can `export CBD_DEFAULT_PROFILE=prod` in your `.bash_profile`.

## Debug

If you want to have more detailed output set the `DEBUG` env variable to non-zero:
```
DEBUG=1 cbd some_command
```

## Update

The tool is capable of upgrade itself:
```
cbd update
```

## Core Containers

- **uaa**: OAuth Identity Server
- cloudbreak
- persicope
- uluwatu
- sultans

## System Level Containers

- consul: Service Registry
- registrator: automatically registers/deregisters containers into consul

## Release Process of Clodbreak Deployer tool

the master branch is always built on [CircleCI](https://circleci.com/gh/sequenceiq/cloudbreak-deployer).
When you wan’t a new release, all you have to do:

- create a PullRequest for the release branch:
  - make sure you change the `VERSION` file
  - update `CHANGELOG.md` with the release date
  - create a new **Unreleased** section in top of `CHANGELOG.md`

Once the PR is merged, CircleCI will:
- create a new release on [github releases tab](https://github.com/sequenceiq/cloudbreak-deployer/releases), with the help of the [gh-release](https://github.com/progrium/gh-release).
- it will create the git tag with `v` prefix like: `v0.0.3`

Sample command when version 0.0.3 was released:

```
export VER=0.0.5
git fetch && git checkout -b release-${VER}
echo $VER > VERSION

# edit CHANGELOG.md

git commit -m "release $VER" VERSION CHANGELOG.md
git push origin release-$VER
hub pull-request -b release -m "release $VER"
```

## Credits

This tool, and the PR driven release, is very much inspired by [glidergun](https://github.com/gliderlabs/glidergun). Actually it
could be a fork of it. The reason it’s not a fork, because we wanted to have our own binary with all modules
built in, so only a single binary is needed.
