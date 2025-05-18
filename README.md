# Monkeysphere

The Monkeysphere project's goal is to extend OpenPGP's web of trust to new areas of the Internet to help us securely identify each other while we work online.

Specifically, monkeysphere currently offers a framework to leverage the OpenPGP web of trust for OpenSSH authentication.

In other words, it allows you to use secure shell as you normally do, but to identify yourself and the servers you administer or connect to with your OpenPGP keys. OpenPGP keys are tracked via GnuPG, and monkeysphere manages the known_hosts and authorized_keys files used by OpenSSH for authentication, checking them for cryptographic validity.

<br />

---

<br />

## Dependencies

 * GnuPG >= 2.1.17
 * find (POSIX or GNU should both work)
 * Perl
 * lockfile-progs or procmail's lockfile
 * Bash
 * OpenSSH's ssh-keygen utility (>= 6.0)
 * base64 (coreutils or fourmilab)

<br />

---

<br />

## Build

To build MonkeySphere; install the following dependencies:

```shell
sudo apt update && \
sudo apt install \
  git-buildpackage \
  libgcrypt20-dev \
  libgpgme-dev \
  libassuan-dev \
  debhelper-compat \
  socat \
  libcrypt-openssl-rsa-perl
```

<br />

Set permissions for the following files; otherwise tests will fail:

```shell
sudo chmod u+x \
  tests/home/testuser/.ssh/proxy-command \
  tests/basic \
  tests/keytrans \
  src/monkeysphere-host \
  src/monkeysphere-authentication \
  src/openpgp2ssh \
  src/monkeysphere
```

<br />

Tests can be ran by going to the root project folder and running:

```shell
cd monkeysphere
make clean
make check
make test
```

<br />

When the tests have successfully ran, you will get a large number of prints which will take approximately 1 minute to run; finishing with:

```console
-------------------- ssh continues below --------------------
debug1: Remote protocol version 2.0, remote software version OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
debug1: compat_banner: match: OpenSSH_8.9p1 Ubuntu-3ubuntu0.10 pat OpenSSH* compat 0x04000000
debug1: Authenticating to testhost.example:22 as 'aetherinox'
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: ssh-ed25519
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: SSH2_MSG_KEX_ECDH_REPLY received
debug1: Server host key: ssh-ed25519 SHA256:tvtnPG6qwKRDudlrzGOYQMjOi4SRTyAfp804Eh2UceU
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
No ED25519 host key is known for testhost.example and you have requested strict checking.
Host key verification failed.
##### ssh connection test PASSED. returned: 255

##################################################
 Monkeysphere basic tests completed successfully!
##################################################
### removing temp dir...
```

<br />

Package can be built by committing the changes:

```shell
make clean
dpkg-source --commit
```

<br />

You will be prompted to update the changelog. Once the changelog has been modified to reflect the newest changes, then build the `.deb`

```shell
gbp buildpackage \
  --git-debian-branch="debian/main" \
  --git-ignore-new \
  --git-upstream-tree="debian/main" \
  -uc \
  -us
```

<br />

The built packages are now one folder back from the root project folder.

<br />

---

<br />

## Install

You can install your current source version by running:

```shell
cd monkeysphere
make install
```

<br />

You will then have the binary packages installed on your system ready to accept input.

<br />

---

<br />

## Troubleshoot

The following lists errors or issues you may potentially come  up against, and solutions to rectify the situation.

<br />

### No Pinentry

When running Monkeysphere tests, you may get the following errors:

```console
gpg: key E00B5EEEBA79B482: public key "Monkeysphere Test Suite Test User (DO NOT USE!!!) <testuser@example.net>" imported
gpg: key E00B5EEEBA79B482/E00B5EEEBA79B482: error sending to agent: No pinentry
gpg: error reading '[stdin]': No pinentry
gpg: import from '[stdin]' failed: No pinentry
gpg: Total number processed: 0
gpg:               imported: 1
gpg:       secret keys read: 1
gpg: no ultimately trusted keys found
```

<br />

To fix, adjust the permissions on two files with:

```shell
sudo chmod u+x \
  tests/phony-pinentry \
  tests/phony-pinentry-nopass
```
