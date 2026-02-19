<p align="center">
  <h1 align="center">unlimited hotspot data</h1>
  <p align="center">
    <b>your phone plan says "unlimited" but caps your hotspot? here's how to get your laptop on that unlimited data too. free. no root. no jailbreak.</b>
  </p>
</p>

<br>

## the problem

most us carriers sell "unlimited" plans that give your phone unlimited data but throttle or cap your hotspot at 5-50gb. after that you're stuck at 600kbps or cut off entirely.

the fix: **make your laptop's traffic look like normal phone traffic** so the carrier doesn't count it as hotspot.

<br>

## how it works

carriers tell hotspot traffic apart from phone traffic using **TTL values** in network packets. tethered devices have a different TTL than your phone's own apps.

[PairVPN](https://pairvpn.com) creates a local vpn tunnel between your phone and laptop over the hotspot. all your laptop's traffic gets routed through the pairvpn app on your phone, so the carrier just sees the app using data. not a tethered device. your laptop piggybacks on your phone's unlimited on-device data and the hotspot cap never gets touched.

<br>

## setup (5 min)

### install pairvpn on both devices

| platform | link |
|----------|------|
| iphone / ipad / mac | [app store](https://apps.apple.com/us/app/id1347012179) |
| android / chromeos | [play store](https://play.google.com/store/apps/details?id=com.pairvpn) / [apk](https://pairvpn.com/bin/) |
| windows | [installer](https://pairvpn.com/bin/PairVPN1916.exe) |

### then

1. turn on your phone's hotspot and connect your laptop to it
2. open pairvpn on your **phone** and set it to **server mode**
3. open pairvpn on your **laptop** and set it to **client mode**
4. on the laptop hit "new server", it shows a 6 digit code, type that into the phone. one time thing
5. hit connect

that's it. verify pairvpn shows **"Tunnel: local"** on the client. if it says "remote" or "relayed" the tunnel isn't going through hotspot and it won't bypass anything.

check your carrier's app or website to confirm hotspot data isn't going up while you use the laptop.

<br>

## good to know

**iphone users** - you gotta keep pairvpn in the foreground with the screen on. ios kills background apps. plug in and disable auto-lock. some people play a silent audio track to keep it alive

**speeds** - expect about 30-40% slower than your phone's raw speed because of vpn overhead. still way faster than 600kbps throttled hotspot. users report ~25mbps through pairvpn on a 40mbps connection

**one device at a time** - pairvpn only supports one client right now. if you need more devices, share your laptop's connection after (internet connection sharing on windows, etc)

**don't stack tethering apps** - no pdanet or easytether alongside pairvpn. you don't need them and they'll conflict

**ignore your phone's hotspot stats** - the built-in counter measures local traffic, not what the carrier sees. always check through your carrier's website or app

<br>

## can carriers detect this?

technically through deep packet inspection, yeah. like if your iphone is suddenly requesting windows updates that's a bit suspicious. but realistically:

- most carriers don't look unless you're pulling absurd amounts of data
- pairvpn encrypts the tunnel so casual inspection won't catch anything
- users report 200gb+ per month without problems
- for normal usage the risk is pretty low

<br>

## which plans work

you need two things:
1. **unlimited on-device data**
2. **any hotspot capability at all** (just enough so the toggle works)

the hotspot cap itself doesn't matter since pairvpn makes everything count as on-device.

| carrier | notes |
|---------|-------|
| **t-mobile** | magenta max / go5g plus, truly unlimited on-device |
| **at&t** | unlimited premium/extra. slightly more aggressive detection |
| **verizon** | unlimited plus/ultimate. known for heavier dpi so don't go wild |
| **visible** | $25/mo unlimited, verizon mvno, solid budget pick |
| **mint mobile** | unlimited works, t-mobile mvno, can get deprioritized |
| **cricket** | at&t mvno, works for most people |
| **us mobile** | depends on plan tier |

if you're shopping specifically for this just grab the cheapest unlimited on-device plan you can find. hotspot allocation is irrelevant.

<br>

## other methods

pairvpn is the easiest free option but not the only approach.

**ttl modification** - carriers spot tethered traffic by its ttl (64 for phone, 63 for tethered since it hops through your phone). some routers can set outgoing ttl to 65 so it arrives at the tower as 64. gl.inet routers with openwrt are popular for this. more complex but can combine with a vpn for extra protection.

**pdanet+** - older tethering app for android. free version blocks https, paid is $7.99. pairvpn does it free.

**usb tethering + vpn** - usb tether with a regular vpn on the phone. encrypts traffic but doesn't always mask ttl values. partial fix.

<br>

## under the hood

```
┌──────────────┐     wifi/usb      ┌──────────────────────┐     cellular     ┌──────────┐
│              │    hotspot link    │                      │      tower       │          │
│    laptop    │◄-----------------►│    phone (server)    │◄---------------►│ internet │
│  (pairvpn    │   vpn tunnel      │  pairvpn re-sends    │  looks like      │          │
│   client)    │   (encrypted)     │  as on-device data   │  phone traffic   │          │
└──────────────┘                   └──────────────────────┘                  └──────────┘
```

instead of your phone's os forwarding packets (which carriers flag as tethering), the pairvpn app itself originates the requests. to the carrier it's just another app on your phone using data. everything's encrypted with 256-bit encryption inside the tunnel.

<br>

## faq

**is pairvpn free?**
yeah, fully free. no accounts, no sign-ups, no subscriptions. support email is zani@pairvpn.com

**works outside the us?**
anywhere carriers separate on-device from hotspot data. the app itself works globally.

**is this legal?**
pairvpn is a legal vpn app. bypassing hotspot restrictions could technically violate your plan's tos but carriers almost never act on it. worst case you get asked to switch plans.

**speeds are slow?**
make sure it shows "Tunnel: local" not "remote" or "relayed". relayed means traffic is going through pairvpn's servers instead of locally. email support@pairvpn.com

**didn't work, hotspot data still got used?**
triple check: (1) tunnel shows "local", (2) server on phone and client on laptop not reversed, (3) you're checking usage via carrier app not your phone's built-in stats

**can this replace home internet?**
yeah but be reasonable. 500gb+/month might draw attention. 100-200gb/month works fine for most people.

<br>

## links

- [pairvpn](https://pairvpn.com) / [hotspot guide](https://pairvpn.com/hotspot) / [install](https://pairvpn.com/install) / [faq](https://pairvpn.com/faq) / [privacy](https://pairvpn.com/privacy)
- [r/Rural_Internet thread](https://www.reddit.com/r/Rural_Internet/comments/1f5wven/does_pairvpn_utilize_4g5g_data_to_make_connection/) / [r/PairVPN](https://www.reddit.com/r/PairVPN/)

<br>

## disclaimer

educational and informational purposes. getting around your carrier's tos is your call and your risk.

---

<p align="center">
  if this saved you money, drop a star. it helps other people find this.
</p>

<br>
<br>

<p align="center">
  <i>inspired by delilah</i> 🌸
</p>
