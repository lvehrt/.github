# `@lovehart` projects

## apparmor.nix

apparmor + apparmor.d patches & tunables which make apparmor more easily 
support nix packages

## firejail.nix

flake that outputs
- seccomp policies for all firejail profiles
- firejail as a r+rx g+rx o-rx executable, to reduce attack surface if it's needed

## pyria.nix

high security nixos + home-manager module. imports packages/settings from
`pyria.rs`,  `apparmor.nix`, and `firejail.nix`

## pyria.rs

the pyria binary, which deals with the following
- hybrid keys (cryptographically combining a secure password & fido2/tpm/etc)
- easier frontend for libcryptsetup
- cli for luks2-with-hybrid-keys setup and unlocking
- run0 wrapper that's a drop-in replacement for sudo

also exports a rust library which offers the following
- hybrid key operations
- luks container operations

## bisphenol

rust utility which allows you to make encrypted overlay containers that
contain all of an app's needed data, cache, etc. unlocked on run, uses
`pyria.rs`'s luks management. exports a cli which you can use
to build packages from various methods, and a nix module which automatically
wraps a package using the rust utility and builds a desktop file with it.

## stronghold

kernel-assisted firejail equivalent, with a much smaller attack surface.
includes a kernel module which wraps page cache invalidation functions, an eBPF
executable which invalidates page cache, verifies binary hashes where 
possible, and optionally blocks execution of non-sandboxed apps that have a
sandbox, a rootful daemon which creates firejail-style sandboxes with verified
policies on command, and a user-side app which asks the daemon to create those
sandboxes.
