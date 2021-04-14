### Primary systems TODO

- Finish porting [this script](https://git.zx2c4.com/wireguard-linux/tree/tools/testing/selftests/wireguard/netns.sh)
  to `./tests/netns.sh` using vnets and epairs.
- Rework locking and epoch lifetimes; come up with consistent set of rules.
- Chop off padding on rx after verifying lengths, so that tcpdump doesn't see
  zeros.
- Shore up vnet support and races/locking around moving between vnets.
- Work out `priv_check` from vnet perspective. (There's no `ns_capable()` on
  FreeBSD, just `capable()`, which makes it a bit weird for one jail to have
  permissions in another.)
- Resize mbufs once at the beginning, and then encrypt/decrypt in place, rather
  than making a new mbuf and copying. (Remember to clear the tags and other
  pieces of metadata before passing it off to udp sending or netisr receiving.)
- Check nonces in serial, rather than in parallel. (This requires taking a
  keypair reference; ncon is working on it.)
- Audit allowedips / radix tree checks, and make sure it's actually behaving as
  expected. (It might be useful to port [this selftest](https://git.zx2c4.com/wireguard-linux/tree/drivers/net/wireguard/selftest/allowedips.c).)
- Make code style consistent with one FreeBSD way, rather than a mix of styles.

### Crypto TODO

- Do packet encryption using opencrypto/ with sg lists on the mbuf, so that we don't need to linearize mbufs.
- Send 25519 upstream to sys/crypto, and port to it.
- Send simple chapoly upstream to sys/crypto, and port to it.
- Port to sys/crypto's blake2s implementation.

### Tooling TODO

- Relicense wg(8) as MIT and integrate into upstream build system.
- Examine possibility of a non-bash wg-quick(8) for sending upstream.