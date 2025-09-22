<h4>Wireguard setup</h4>

Okay, I'm not gonna lie I relied on ChatGPT's help heavily with setting up Wireguard. However, it's all because of a tiny error I didn't catch that took me forever to find. So I followed this guide: https://www.tech2geek.net/how-to-set-up-a-wireguard-vpn-on-linux-step-by-step-guide/ which was great. It would have worked out of the box no question, except for one thing.

I had tried to setup Wireguard a few days ago, and must have had a typo or something somewhere, because it wasn't working. In my wg0.conf file, I had put my server's private key, and saveConfig = True. Unbenownst to me, this meant that anytime I changed this file, it would reset it to the saved state when I reset wireguard, meaning it used the old private key that I got rid of in my quest to start from scratch with a new guide.

```
[Interface]
Address = ...
saveConfig = True :(
PostUp = ...
PostDown = ...
ListenPort = ...
PrivateKey = <server's private key>
```

Anyway, once I figured that out, it all worked great! I now can turn on a VPN on my phone or laptop, and access my server from anywhere in the world. It feels surreal and insanely cool. There's something about this that feels so much more grounded and meaningful than any random project I've done in CS. I'm sure I'd feel differently working on a real project in a job or internship, but this is a cool feeling.
