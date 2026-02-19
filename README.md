<p align="center">
  <h1 align="center">unlimited hotspot data</h1>
  <p align="center">
    <b>your carrier's "unlimited" plan caps your hotspot? cute. here's how to not deal with that anymore. free. no root. no jailbreak. you're welcome.</b>
  </p>
</p>

<br>

## the problem

so your carrier charges you for an "unlimited" plan and then has the audacity to throttle your hotspot after like 15gb. make it make sense. you're paying for unlimited data but the second your laptop touches it you're capped at 600kbps like it's 2008.

anyway. the fix is embarrassingly simple: **make your laptop's traffic look like normal phone traffic** so the carrier doesn't even know your hotspot is involved.

<br>

## how it works

carriers can tell the difference between your phone using data and a laptop tethered to your phone using data. they do this through **TTL values** in packets. tethered traffic has a different TTL. riveting stuff i know.

[PairVPN](https://pairvpn.com) makes a local vpn tunnel between your phone and laptop over the hotspot. your laptop's traffic gets routed through the pairvpn app on your phone so the carrier just thinks the app is using data. not a whole laptop. your hotspot cap doesn't get touched. that's literally it.

<br>

## setup (takes like 5 min, you'll live)

### install pairvpn on both devices

| platform | link |
|----------|------|
| iphone / ipad / mac | [app store](https://apps.apple.com/us/app/id1347012179) |
| android / chromeos | [play store](https://play.google.com/store/apps/details?id=com.pairvpn) / [apk](https://pairvpn.com/bin/) |
| windows | [installer](https://pairvpn.com/bin/PairVPN1916.exe) |

### then

1. turn on your phone's hotspot, connect your laptop to it. groundbreaking, i know
2. open pairvpn on your **phone**, set it to **server mode**
3. open pairvpn on your **laptop**, set it to **client mode**
4. on the laptop hit "new server", it gives you a 6 digit code, type it into the phone. you only do this once
5. hit connect

done. verify it shows **"Tunnel: local"** on the client. if it says "remote" or "relayed" it's not working and you probably did something wrong. go back and reread step 2 and 3.

check your carrier's app to make sure your hotspot data isn't going up. if it's not, congrats, you can read instructions.

<br>

## things that'll annoy you

**iphone users** - you have to keep pairvpn in the foreground with the screen on because ios loves killing background apps. yes it's annoying. plug in, disable auto-lock, deal with it. some people play a silent audio track to keep it alive which is unhinged but works

**speeds** - you'll lose about 30-40% speed from vpn overhead. so like 25mbps instead of 40mbps. still better than the 600kbps your carrier was graciously offering you after your cap

**one device at a time** - pairvpn only does one client connection. if you want more devices online, share your laptop's connection after. figure it out

**don't use other tethering apps with this** - no pdanet, no easytether, nothing. they'll conflict and then you'll come complain somewhere and nobody wants that

**your phone's hotspot stats will look scary** - ignore them. that counter measures local traffic not what the carrier sees. check your actual usage through your carrier's app

<br>

## can carriers catch you

i mean technically? through deep packet inspection? like if your iphone starts requesting windows updates that's a little weird. but honestly:

- most carriers do not care unless you're downloading the entire internet
- pairvpn encrypts the tunnel so they can't casually snoop
- people have done 200gb+ a month and nothing happened
- you're probably fine. relax

<br>

## which plans work

you need:
1. **unlimited on-device data**
2. **literally any hotspot capability** (just enough that the toggle exists)

the hotspot cap is irrelevant because pairvpn makes everything look like on-device data. so stop overthinking it.

| carrier | notes |
|---------|-------|
| **t-mobile** | magenta max / go5g plus, truly unlimited on-device |
| **at&t** | unlimited premium/extra. at&t tries harder to detect stuff but whatever |
| **verizon** | unlimited plus/ultimate. heavier on the dpi so maybe don't do 500gb |
| **visible** | $25/mo unlimited verizon mvno. budget queen choice |
| **mint mobile** | t-mobile mvno, works fine, might get deprioritized |
| **cricket** | at&t mvno, works for most |
| **us mobile** | depends on your tier |

if you're plan shopping just get the cheapest unlimited on-device data you can find. hotspot allocation literally does not matter.

<br>

## other ways to do this

pairvpn is the easiest and it's free but sure there are alternatives if you want to make your life harder.

**ttl modification** - phone traffic has ttl 64, tethered has 63 because of the extra hop. some routers can set outgoing ttl to 65 so it hits the tower as 64. gl.inet routers with openwrt are popular for this. more work than pairvpn but you do you.

**pdanet+** - android tethering app. free version blocks https which is hilarious. paid is $7.99. or just use pairvpn for free.

**usb tethering + vpn** - usb tether with a vpn on the phone. encrypts stuff but doesn't always fix the ttl. half measure.

<br>

## under the hood

for the nerds:

```
┌──────────────┐     wifi/usb      ┌──────────────────────┐     cellular     ┌──────────┐
│              │    hotspot link    │                      │      tower       │          │
│    laptop    │◄-----------------►│    phone (server)    │◄---------------►│ internet │
│  (pairvpn    │   vpn tunnel      │  pairvpn re-sends    │  looks like      │          │
│   client)    │   (encrypted)     │  as on-device data   │  phone traffic   │          │
└──────────────┘                   └──────────────────────┘                  └──────────┘
```

instead of your phone's os forwarding packets (which carriers flag as tethering), pairvpn originates the requests itself. carrier just sees another app using data. 256-bit encrypted tunnel. moving on.

<br>

## faq

**is pairvpn free?**
yes. completely. no accounts no sign-ups no subscriptions no catch. support is zani@pairvpn.com

**works outside the us?**
anywhere carriers separate on-device from hotspot data. so yes most places.

**is this legal?**
pairvpn is a legal app. could it violate your carrier's tos? maybe. do carriers ever actually do anything about it? almost never. worst case they ask you to switch plans. you'll survive.

**it's slow?**
check that it shows "Tunnel: local" not "remote" or "relayed". if it's relayed your tunnel isn't local and that's your problem right there. email support@pairvpn.com

**didn't work, hotspot data still went up?**
(1) does it say "local"? (2) is server on the phone and client on the laptop and not backwards? (3) are you checking via your carrier's app and not your phone's built-in stats? if you said no to any of those, there's your answer.

**can this replace home internet?**
technically. don't be insane about it though. 100-200gb/month is fine. 500gb+ and you might get a look.

<br>

## links

- [pairvpn](https://pairvpn.com) / [hotspot guide](https://pairvpn.com/hotspot) / [install](https://pairvpn.com/install) / [faq](https://pairvpn.com/faq) / [privacy](https://pairvpn.com/privacy)
- [r/Rural_Internet thread](https://www.reddit.com/r/Rural_Internet/comments/1f5wven/does_pairvpn_utilize_4g5g_data_to_make_connection/) / [r/PairVPN](https://www.reddit.com/r/PairVPN/)

<br>

## disclaimer

educational and informational. your carrier's tos is between you and god.

---

<p align="center">
  if this saved you money, star it or whatever. helps other people find it.
</p>

<br>
<br>

<p align="center">
  <i>inspired by delilah</i> 🌸
</p>
