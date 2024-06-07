% Raising Pwnagotchi
% Joe Cathell <kamikazejoe@gmail.com>
%![](static/qrcode.png)<br/>Talk: [${TALK_URL}](${TALK_URL})<br/>Repo: [${REPO_URL}](${REPO_URL})


# What is a Pwnagotchi?

<img src="../../_resources/99e3f9423c38cacf11215a85eb5ca496.png" alt="99e3f9423c38cacf11215a85eb5ca496.png" width="391" height="293">

A virtual pet that eats Wi-Fi handshakes.

::: notes

Meet Pwnachu and Sniffizard (add picture)

They are virtual pets that eats Wi-Fi.

Because everyone knows good free range organic Wi-Fi is low calorie and part of a complete breakfast

:::

# What is a Pwnagotchi, REALLY?

A pocket sized tool to help automate auditing Wi-Fi.

::: notes

Pwnagotchi was developed by a hacker called EvilSocket

While spending the summer of 2019 travelling the US, he began tinkering with a way to automate certain Wi-Fi attacks. Namely sending deauth packets and collecting handshakes.

As he continued to tinker with it, some basic AI was incorporated for optimization, and then slapped a face on it.

Thus the pwnagotchi was born.

When EvilSocket shared his efforts online, a community quickly sprung up around the device. Which has since pretty much taken over development of the project.

:::

# What does a Pwnagotchi do?

- Sniffs surrounding Wi-Fi
- Captures WPA handshake packets
- Uses reinforcement learning
- Looks adorable doing so

::: notes

It hops across the Wi-Fi spectrum hunting for handshake packets to capture  
It uses some very basic "AI" to optimize it's ability to capture handshakes  
And it comes in an adorable little package

I know "AI" is a pretty big buzzword these days, so please excuse it's use in this talk.  
This is a very simple reinforcement learning neural network that does very simple things.  
This isn't the generative spicy autocorrect that all the tech bros have a hard-on for right now.

:::

# Does it do anything else?

- PwnGrid Network
- Interaction with other Pwnagotchis

::: notes

Optionally it can connect to what's called the PwnGrid network, which gamifies the process.

It also will talk with other Pwnagotchis, allowing you to distribute the work.

:::

# How do I get my own Pwnagotchi?

- Raspberry Pi
- Micro SD Card
- Display
- Battery
- Optional USB Wi-Fi Adapter

::: notes

Easy, you build one.

At minimum you want a Raspberry Pi Zero 2 W.  
Pi's 3, 4 and 5 are supported as well, but there is a tradeoff in power usage in exchange for the beefier hardware.

Of course you are going to need a Micro SD card to go along with the Raspberry Pi.  
The Pwnagotchi does a lot of writing to the card, so you are going to need something reliable.

There's not an official list of supported displays,  
but each supported display has a module written for it in the Github repo.  
They are pretty clearly labeled.

Now if your Pi doesn't support Wi-Fi on the 5 GHz band, such as the Pi Zero, you may want to add a USB Wi-Fi Adapter that can support the full spectrum.

:::

# My Pwnagotchi's Hardware

- Raspberry Pi Zero 2W
- Samsung Evo Select/Plus
- Waveshare 2.13 V3 Hat
- PiSugar v2 Battery Pack

::: notes

I stuck with a Raspberry Pi Zero 2W for portability

My goto Micro SD cards are the Samsung Evo series.  
They've been extrely reliable for projects requiring frequent writes.

Waveshare epaper displays are generally recommended by the community because they are low power.  
I'm using Waveshare 2.13 inch e-paper hat V3 on Pwnachu, which is my primary pwnagotchi.  
But Sniffizard is using my Flipper Zero. Which was supported through a plugin

I'm using a PiSugar battery pack, which is pretty commonly recommended.  
I get about 4 hours of use with it.  
It also requires a plugin, and some power managment software be installed  
Though it was only took like 4 commands and the rest was scripted.  
The downside is that though the source is available on github, the install script downloads all the binaries from a CDN in China.  
I didn't see a real risk, as I'm not storing any sensitive data on this device and requires manual intervention to put it online. But judge the risk for yourself.

:::

# My Pwnagotchi's Hardware

<img src="../../_resources/e002c9269af7104082ad7a3b80a38362.png" alt="e002c9269af7104082ad7a3b80a38362.png" width="725" height="496">

::: notes

Most of these choices however were more out of convenience rather than community recommendations

The makers of the PiSugar have a Pwnagotchi kit readily available on Tindie

It includes an SD card (which I replaced), the Waveshare display, the PiSugar, and a nicely printed case.

You can also get the kit with a Pi Zero if you can't be bothered by a trip to Microcenter

:::

# Pwnagotchi Software

- Raspbian Base
- Nexmon for monitoring
- Bettercap
- Pwnagotchi Service

::: notes

Notes go here

:::

# Nexmon

Enables monitoring mode on broadcom chipsets

::: notes

Nexmon is a firmware patching framework made for Broadcom and Cypress Wi-Fi chips.

This is what enables your pwnagotchi to set it's Wi-Fi to monitor mode and conduct wireless attacks.

It's not normally part of Raspbian, so it had to be added to the OS image.

:::

# Bettercap

- "Swiss Army Knife" for various wireless attacks
- Wi-Fi
- Bluetooth LE
- Wireless HID
- Recon and MitM Attacks

::: notes

Bettercap is the tool that conducts the actual Wi-Fi attacks.

Under normal operation it runs in the background and is managed by the Pwnagotchi service.

However if you set the pwnagotchi in manual mode (we'll discuss modes later) you can access Bettercap through it's web interface.

- Boot to MANUAL mode.
- http://pwnagotchi.local
- pwnagotchi:pwnagotchi
- Change them in `/usr/local/share/bettercap/caplets/pwnagotchi-*.cap`, and `/etc/pwnagotchi/config.toml`

:::

# Bettercap Attacks

- Deauth
- Handshake Capture
- Client-less PMKID Attack

::: notes

The two main attacks conducted by Bettercap are, as previously mentioned:

- Sending deauthing packets
- Capturing Wi-Fi handshakes
- Partial handshake captures

:::

# Deauthing

- Denial-of-service attack
- Part of the 802.11 protocol
- Forces devices to disconnect
- Positions attacker for next attack

::: notes

A "Wi-Fi Dauthentication Attack", or Deauthing, is a denial-of-service attack that exploits the 802.11 protocol itself.

You basically a signal that says "I'm disconnecting from the network now".

:::

# Deauthing

<img src="../../_resources/632620805f29b0657e6a3430d7efdb38.png" alt="632620805f29b0657e6a3430d7efdb38.png" width="375" height="546">

::: notes

The 802.11 protocol contains a provison for a what's called a dauthentication frame.

Deauth packets don't require any encryption, regardless of the security setting on the access point.

So an attacker can easily send a deauth packet while spoofing a target's MAC address, forcing them to be disconnected.

:::

# Dauthing

![d7a77b39aa7d436429ed8b4d4327a3e4.png](../../_resources/d7a77b39aa7d436429ed8b4d4327a3e4.png)

::: notes

The result is the target is kicked off the network, at least temporarily.

When the target attempts to reconnect to the network, it will have to reauthenticate. This is usually in the background and invisible to the user.

An attacker is now position to capture the autentication handshake.

Fun fact:  
The Federal Communications Commission has fined hotels and other companies for launching deauthentication attacks on their own guests; the purpose being to drive them off their own personal hotspots and force them to pay for on-site Wi-Fi services

:::

# Handshake Capturing

<img src="../../_resources/a10a0bcb513fe41c1cb8292886da40e3.png" alt="a10a0bcb513fe41c1cb8292886da40e3.png" width="446" height="504">

::: notes

When connecting to a Wi-Fi access point with WPA enabled, your device and the access point go through what's called a 4-way Handshake

Four packets are exchanged between the client and access point.

These are used to derive the session keys from the Wi-Fi password

Once this exchange is completed and the keys are generated, your device is now securely connected to the Wi-Fi network.

The PMK, which is the Wi-Fi password, is used to derive the PTK using PBKDF2 (Password based key derivation function 2) which uses HMAC-SHA1 to encode it along with the SSID and MAC addresses

If we capture the entire handshake, we use the fact we know the MAC addresses and SSID to brute force the remaining information, i.e. the password.

This of course very generalized and lacking all the math involved.

:::

# Client-less PMKID Attack

- Handshakes are sometimes captured without deauthing
- Sometimes a partial handshake is enough
- Many routers include PMKID to the EAPOL Frame

::: notes

Sometimes you get lucky and can capture a handshake that's already in progress

In many cases even a partial handshake from the access point can be useful to cracking Wi-Fi

Many modern routers inlcude a an extra field in the EAPOL frame called "Robust Security Network" or RSN.

This field includes what's called a PMKID.

This is a hash of the PMK, the PMK Name, and the AP and Client MACs.

It's meant as a way for Access Points to keep track of who has what PMK for each session.

But the PMK Name and the MACs are constants, so this hash can be used to derive the Wi-Fi password.

:::

# Pwnagotchi Services

- Provides the UI
- Manages Bettercap attacks
- Runs the AI
- Access PwnGrid

::: notes

The Pwnagotchi service itself is the platform the coordinates everything

It provides a web interface to view the dashboard, which is mirrored on the display

You can check in with the PwnGrid

Manage your plugins

And when enabled, is driven by the AI.

:::

# OS Images

- Original EvilSocket Image
- JayoFelony Image
- Aluminium Ice Image

::: notes

Since all of the software is open source, you could certainly jump through all the hoops of building a disk image from scratch and spend countless hours getting it working right.

Fortunately there are several pre-made images avaialble.

There is the Original image by EvilSocket.

- No longer developed
- Outdated
- Bugs affecting AI mode

The most popular image currently seems to be JayoFelony's Image.

- Better CPU usage
- Active Development
- Optimised
- More default plugins

Another popular image is by Aluminium Ice.

- Also activley developed
- Bettercap and Nexmon compiled from source
- Updated dependecies
- Some crash prevention

I went with JayoFelony's, as it was the most current and easiest to get running.  
I dabbled a bit with Aluminium Ice's, but it just wasn't as simple to get running.

:::

# How to use the Pwnagotchi

- Modes
- Moods
- AI
- Making Friends
- PwnGrid
- Cracking Wi-Fi
- Customization

::: notes

Using the Pwnagotchi is deceptively simple at first glance.

Once it's up and running, you just walk around with it and let it do it's thing.

But there's a lot more going on behind the scenes than at first glance.

:::

# Pwnagotchi's Face

<img src="../../_resources/78035c6bdf480bc8a2ddd6be5dfcb28d.png" alt="78035c6bdf480bc8a2ddd6be5dfcb28d.png" width="536" height="255">

::: notes

This i the Pwnagotchi dashboard, or the face of the pwnagotchi.

From here we can observe what the pwnagotchi is doing at any given time.

The top bar is a status bar.

It tells us where it's scannong on the wifi, how many AP's it sees. How long the system has been up. Battey status and network connectivity is shown up here when available as well.

In the center of the dashboard we see the Pwnagotchi's mood, the status of whatever action it's currently taking, as well as if it sees any other Pwnagotchis in the vicinity.

Along the bottom we see how many handshakes have been capture during it's uptime, as well as the total number of captured handshakes recorded.

Following that is the SSID most recently captured handshake

And to the far right we see what mode the Pwnagotchi is currently operating in.

:::

# Available Operating Modes

- Manual
- Auto
- AI

::: notes

Pwnagotchi's have three basic operating modes.

In Manual mode, the Pwnagotchi doesn't do anything on it's own. No sniffing, no dauthing, nothing.

You use this mode when you are updating or backing up your Pwnagotchi, or wanting to use Bettercap on it's own without all the Pwnagotchi automation.

Auto mode how a Pwnagotchi starts out.

It sniffs and captures handshakes in a default manner defined by the configuration files.

It's perfectly functional at this point, but doesn't have any of the optimization provided by the AI.

When you first power on, the Pwnagotchi will default to this mode while the AI's neural network is loading, which can take up to 30 minutes.

AI mode is where it starts to shine.

Once the neural network has finished loading and is function, it will begin to tweak it's algorithm to optimize where and when it deauths, sniffs, and captures packets for maximum pwnage.

:::

# Moods

```
(⇀‿‿↼) sleeping
( ⚆_⚆), (☉_☉ ) observing (neutral mood)
(°▃▃°) intense
(⌐■_■) cool
(•‿‿•) happy

```

::: notes

Moods of the Pwnagotchi, as the name suggests, reflects how happy, sad, or bored it is.

The more exciting Wi-Fi activity there is around it,  
the more handshakes it can capture,  
the happier the Pwnagotchi is.

If activity is pretty slow or sparse, Pwnagotchi will get sad and bored.  
When this happens, the best way to make it happy again is to take it for a walk where there is more Wi-Fi activity.

:::

# The AI

- A2CS - Advantage Actor Critic
- Optimizes parameters

::: notes

The AI being used by the Pwnagotchi is a method of Reinforcement Learning called Advantage Actor Criti, or A2CS.

As the Pwnagotchi continously runs, the settings are optimized over time based on what the AI has learned.

:::

# Advantage Actor Critic

- The Actor
- The Critic
- Advantage

::: notes

The Reinforcement Learning agent has three components.

The Actor tells the agent which choices to make.  
Like someone trying to help you play a video game by telling you which buttons to press.

The Critic provides feedback to the agent on those choices.  
It keeps score and tells you if that last move you made was a good move or a bad move and if you should try something else next time.

The Critic also provides feedback on the Advantage. It's not enough to know if a move was good or bad, but how much better or worse a move was compared to what was expected.  
Like if the move was only suppose to knock a few hit points off an enemby, but actually ended up being a critical blow.

This feed back occurs every few actions, repeadedly review moderate sample sizes. Like looking around every now and then to make sure you are going the right direction.

Compared to a Monte Carlo system, which waits until you've completed the whole game before it critiques your actions. Or a One-step system which reviews choices after every step.

In a real world scenario like the Pwnagotchi exists in, One-step systems are too slow to be efficient, and there's no real concievable end of the game for a Monte Carlo system to reach before it can analyze the actions made.

For the developer in the crowd:  
Pwnagotchi is using a python library called Stable Baselines, which is a re-implementaiton of Open AI's Baselines library.

:::

# Pwnagotchi's Main Loop

```
# main loop
while True:
    # ask bettercap for all visible access points and their clients
    aps = get_all_visible_access_points()
    # loop each AP
    for ap in aps:
        # send an association frame in order to grab the PMKID
        send_assoc(ap)
        # loop each client station of the AP
        for client in ap.clients:
            # deauthenticate the client to get its half or full handshake
            deauthenticate(client)

    wait_for_loot()
```

# What Gets Tweaked

- Nearly 20 different "Personality" settings
- Wait Times
- Timeouts
- Signal Strength
- Channel Hopping Order
- Frequency of Attacks

::: notes

Since every environment is different, there's not optimal set of parameters.

Different settings are best when you are wakling around a downtown shopping district verus being stationary in an populated office building.

There are almost 20 different settings specifically dictating a Pwnagotchi's personality

Everything from Wait times, to time outs, signal strenths, etc.

:::

# The Reward Function

```
# state contains the information of the last epoch
# epoch_n is the number of the last epoch
tot_epochs = epoch_n + 1e-20 # 1e-20 is added to avoid a division by 0
tot_interactions = max(state['num_deauths'] + state['num_associations'], state['num_handshakes']) + 1e-20
tot_channels = wifi.NumChannels

# ideally, for each interaction we would have an handshake
h = state['num_handshakes'] / tot_interactions
# small positive rewards the more active epochs we have
a = .2 * (state['active_for_epochs'] / tot_epochs)
# make sure we keep hopping on the widest channel spectrum
c = .1 * (state['num_hops'] / tot_channels)
# small negative reward if we don't see aps for a while
b = -.3 * (state['blind_for_epochs'] / tot_epochs)
# small negative reward if we interact with things that are not in range anymore
m = -.3 * (state['missed_interactions'] / tot_interactions)
# small negative reward for inactive epochs
i = -.2 * (state['inactive_for_epochs'] / tot_epochs)

reward = h + a + c + b + i + m
```

::: notes

After each loop iteration, a score is calculated on how well the settings did.

Basically the more Access Points seen and handshakes captured, the higher the score.

:::

# Making Friends

![dfa90d9ac3038909993b1ab8b0a78302.png](../../_resources/dfa90d9ac3038909993b1ab8b0a78302.png)

::: notes

I've mentioned previously about your Pwnagotchi making friends. What do I mean by this?

While scanning, deauthing, capturing, and everything else, your Pwnagotchi periodically sends out advertisement packets on whatever Wi-Fi channel it's scanning.

It also listens for other advertisement packets while it scans.

This allows Pwnagotchis to notice and recognize each other when they are within range.

Once they do, they become friends. And the more they see each other, the better friends they become.

And friendly Pwnagotchis tend towards having a happier mood.

Pwnagotchis that are friends also start exchanging information with each other.  
Information like it's name, signal strength, number of pwned networks, and current channels being scanned.

This exchange of information will being to influence a Pwnagotchi's behavior.

Friendly Pwnagotchis will start to split up the spectrum by dividing channels among them, allowing them to distribute the work and capture more handshakes collectively.

So if you happen to be conducting an audit of a campus wide Wi-Fi network, having a group of Pwnagotchis at your disposal would be an asset.

:::

# PwnGrid

- Ranking
- Messaging

::: notes

PwnGrid is service online that your Pwnagotchis can connect to.

It gameifies the Pwnagotchis activity by reporting information about the Pwnagotch for ranking.

PwnGrid also facilitates a way to send encrypted messages between Pwnagotchi owners.

Though the original PwnGrid is still active, due to the orginal Pwnagotchi projects lack of development, the current forks have moved to oPwnGrid. Which is a re-implementation of the original PwnGrid API.

Grid functionality can also be disabled on your Pwnagotchi, if you don't want to participate.

:::

# PwnGrid Rankings

![d4c2cbc2c3bada02a6b2224d5e5f74bd.png](../../_resources/d4c2cbc2c3bada02a6b2224d5e5f74bd.png)

::: notes

When connected to the Internet, each Pwnagotchi submits data about it's neural network as a form of ranking.

This provides a way to judge how many networks a Pwnagotchi has "pwned" without relying on actual network data that could be spammed.

:::

# PwnMail

<img src="../../_resources/bd2a655754cd147cf8e3f4f3056ac76a.png" alt="bd2a655754cd147cf8e3f4f3056ac76a.png" width="725" height="352">

Sending End-to-End Encrypted Messages

::: notes

As mentioned a few times so far, a Pwnagotchi can send and recieve messages to other friendly Pwnagotchis, functioning as a sort of "Crypto-Pager"

Each message is sent through the PwnGrid system by being first encrypted with an RSA Public Key.

Only the recipient Pwnagotchi has the private key.

Your PwnMAIL address to have messages sent to is the Pwnagotchis cryptographic fingerprint (the SHA256 hash of the public key).

This is a little combersome to use of course

If everyone is interested, we'll see how that's done in the Demo

:::

# Cracking Wi-Fi

- What to do with Handshakes
- John the Ripper or Hashcat

::: notes

So we've built our Pwnagotchi. Fed it. Taken it for walks.

Now what? What do we do with all these handshakes it's captured.

The little Pi Zero definitely isn't powerful enough to do any kind of brute force password cracking, so we have to get them off there.

You can either connect to the Pi and copy them over SSH, or simply plug the Micro SD card into your computer.

Once we have those transferred, we want to use either John the Ripper or Hashcat try and crack them.

In general I suggest John the Ripper for when you only have a CPU.  
If you have a dedicated GPU on hand, I suggest going with Hashcat.

During the demo we'll be using John because that's all my laptop can handle.

:::

# Cracking Wi-Fi Online

- https://banthex.de/
- https://www.onlinehashcrack.com/
- https://wpa-sec.stanev.org/

::: notes

Alternatively there are some online services as well that will do this for you in some cases.

In no particular order, there is...

There are also plugins that will automatically load your handshakes to these services from the Pwnagotchi

:::

# Documentation Sources

- [Pwnagotchi.ai - Original Wiki](https://pwnagotchi.ai)
- [Pwnagotchi.org - New Wiki](https://pwnagotchi.org)
- [Pwnagotchi Sub-Reddit](https://www.reddit.com/r/pwnagotchi/)

# Questions?

# Demo

::: notes

- Demo AP setup or ARGuest
- Connecting to Pwnagotchi
- Web UI
- Send/Receive PwnMail
    - `sudo pwngrid -send ADDRESS -message "Hello World!"`
    - `sudo pwngrid -send ADDRESS -message @/path/to/file.jpg`
    - `sudo pwngrid -inbox`
- Transfering Hashes
- John The Ripper
    - Format hashes
    - `wpapcap2john filename.pcap > filename.hash`
    - `for filename in *; do wpapcap2john ${filename} > ${filename}.hash; done`
    - `john --wordlist=./wordlist.txt ARMembers_e663daaa4d6a.pcap.hash`
    - `john --show ARMembers_e663daaa4d6a.pcap.hash`

:::

---

Joe Cathell <kamikazejoe@gmail.com>

![](static/qrcode.png)

Talk: [${TALK_URL}](${TALK_URL})

Repo: [${REPO_URL}](${REPO_URL})
