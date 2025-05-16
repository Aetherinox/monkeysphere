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

