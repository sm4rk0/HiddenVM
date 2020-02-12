# HiddenVM

**HiddenVM** is an innovation in computing privacy.

Imagine you're entering a country at the airport. The border agents seize your laptop and force you to unlock it, so that they can violate your privacy, treat you like a criminal, and [insult your humanity](https://www.reddit.com/r/privacy/comments/epblc8/australian_border_employee_hands_phone_back_to/). Is that the world you want to live in?

Whether you use Windows, macOS or Linux, now there's a tech solution for real privacy: **HiddenVM**.

**HiddenVM** is a simple, one-click, free and open-source Linux application that allows you to run Oracle's open-source [VirtualBox software](https://virtualbox.org) on the [Tails operating system](https://tails.boum.org).

This means you can run almost any OS in a VM inside the most anti-forensic computing environment in the world. Works where Tails does.

The VM even uses your full-speed pre-Tor Internet by default, while leaving the outer Tor connection in Tails undisturbed.

To ensure anti-forensic deniability of your VMs, you can place your persistent HiddenVM installation - containing all VirtualBox binaries, your VMs, and HiddenVM itself - in a [hidden VeraCrypt volume](https://www.veracrypt.fr/en/Hidden%20Volume.html), and only mount it when using the amnesic Tails.

When your computer is turned off, all anyone can plausibly see is a blank Tails USB and a hard drive full of meaningless data, or a bootable decoy OS partition that you can create.

How does it feel to have no forensic trace of your entire operating system - whether it's Windows, macOS or Linux - ever touch your hard drive?

Now you can find out. True privacy in computing, finally here.

## Installation and usage

- Boot into [Tails](https://tails.boum.org) on your computer and set an [administration password](https://tails.boum.org/doc/first_steps/startup_options/administration_password/index.en.html) for your session.

- Don't use the [persistent volume feature](https://tails.boum.org/doc/first_steps/persistence/index.en.html).

- Create and mount a deniable, secure storage environment on internal or external media such as a [VeraCrypt](veracrypt.fr/en) volume.

- **[Download our latest release ZIP](https://github.com/aforensics/HiddenVM/releases)** and extract the archive.

- Run our AppImage file from the file browser.

- Choose to store HiddenVM in your secure storage and it will download all binaries to launch VirtualBox.

- After first-time install you can use HiddenVM offline and each AppImage launch takes about 2 minutes.

## How can I trust the HiddenVM AppImage file?

### You don't have to. Inspect our code:

- Open a Terminal and `cd` to the folder containing our AppImage.

- Run `mkdir inspect && sudo mount HiddenVM-*-x86_64.AppImage inspect -o offset=188456`

- Every file in the mounted folder can be inspected with a text editor. To search for IP addresses or web domains that HiddenVM could try to phone home to and violate your privacy, use [Searchmonkey](http://searchmonkey.embeddediq.com) (`sudo apt install searchmonkey`) to recursively search for `\.\S` in the mounted folder's files.

- Once you trust the current version of HVM, when new releases arrive you can track code changes by using [Meld](https://meldmerge.org) (`sudo apt install meld`). Drag and drop the old and new folders together into *Meld*, and any code differences will be highlighted.

### Or generate your own AppImage from our code after inspecting it:

1. `git clone https://github.com/aforensics/HiddenVM.git`

2. `cd HiddenVM/appimage`

3. `./make-appimage.sh` (The script will download **appimagetool** from [AppImageKit](https://github.com/AppImage/AppImageKit) if need be.)

Find your own AppImage in the `target` subdir.

## FAQs / Warnings

### What type of person might use HiddenVM?

Like Tor and Tails, HiddenVM is intended for a wide range of use cases around the world. In our digital age of increasing surveillance and control, we need tools to keep true privacy and freedom alive.

If you are a political dissident in a country under totalitarian rule, for someone in your situation there has never been a robust tech solution to truly hide and protect your data in a convenient way. Our tool may help you.

We are aligned with Tails and Tor projects in our intentions and promotion of how our software can and should be used.

### What guest operating systems work with HiddenVM?

So far we have successfully tested Windows 10, macOS Mojave, Linux Mint, Ubuntu, Xubuntu, Fedora, and Whonix. Anything that works in VirtualBox should be compatible. Our Wiki will have how-to's and links for specific OSes. Please contribute your findings in our [subreddit](https://reddit.com/r/HiddenVM).

### How much RAM do I need?

Using VMs in Tails uses a lot of RAM, because Tails already runs entirely in RAM. We recommended at least 16 GB in your machine, but your mileage may vary.

### Why is HiddenVM taking more than the normal 2 minutes to launch?

The first time you run HiddenVM, the install may take anywhere from several minutes to more than half an hour because it needs to download all the necessary software for the first time. After that it caches everything offline for a much quicker usual 2-minute launch time.

Every 7 days, if you're connected to the Internet HiddenVM will do an `apt-get` update to check repositories like VirtualBox, and will download new updates if available. Sometimes you can get connected to a very slow Tor circuit in Tails. Close off HiddenVM's Terminal window and restart Tails to hopefully get connected to a faster circuit.

Every time you do a Tails and HiddenVM upgrade, the first time after that will almost always need to install new package versions, taking around 5 minutes or longer. Then it returns to the usual 2 minutes.

### Can I use HiddenVM offline?

Yes. In fact it may be possible to use HVM offline for extended periods of even months at a time, if you never update Tails or HVM during such periods.

We can't guarantee this, but limited testing by the team has confirmed this being possible for at least a month.

As soon as you connect to the Internet, HVM may upgrade its cached software and you may have to upgrade to the latest HVM on our Github as well as Tails itself, but after all software is updated and verified as in sync by HVM, it may be possible to use it offline for a long period once more.

### 'Extras' and 'Dotfiles' feature

HiddenVM allows you to fully automate the customization of your Tails environment at each launch by performing system settings modifications or loading additional software including persistent config files for it.

Go to 'extras' folder in your HiddenVM and rename `extras-example.sh` to `extras.sh`. Any lines you add will be performed as bash script code at the end of each subsequent HVM launch, right after it opens VirtualBox.

Some examples:

```
sudo apt-get install autokey-gtk -y #Install a popular Linux universal hotkeys tool
```

```
nohup autokey & #Launch that same Linux universal hotkeys tool that Extras just installed
```

```
gsettings set org.gnome.desktop.interface enable-animations false #Turn off sluggish GNOME animations
```

```
#Disable JavaScript in Tails Tor Browser and set its 'Security Level' to 'Safest' 
CONFIG_FILE=/home/amnesia/.tor-browser/profile.default/user.js
echo -e "user_pref(\"extensions.torbutton.security_slider\", 1);" >> "$CONFIG_FILE"
echo -e "user_pref(\"javascript.enabled\", false);" >> "$CONFIG_FILE"
```

Eventually we will have a Wiki page with many Extras examples. Please contribute ideas. The installation and launching of a pre-VirtualBox VPN could be possible.

Warning: Make sure your commands work or it may cause HVM to not fully exit its Terminal or to produce errors.

**Dotfiles:** Inside 'extras' is a 'dotfiles' folder. Place any files and folder structures in there and HVM will recursively symlink it into the Tails session's Home folder at `/home/amnesia`. This feature is very powerful and by putting a *.config* folder structure in there you can have all additional software settings pre-loaded before they're install via Extras.

### Why shouldn't I use Tails' official persistent volume feature?

Tails' [Additional Software](https://tails.boum.org/doc/first_steps/additional_software/index.en.html#index1h2) feature disturbs HiddenVM's complicated `apt-get update` sorcery that achieves our VirtualBox-installing breakthrough.

More importantly, our intention is for HVM's virtual machines to be truly 'hidden', i.e. forensically undetectable. This is the first time you can emulate VeraCrypt's Windows [Hidden OS](https://www.veracrypt.fr/en/VeraCrypt%20Hidden%20Operating%20System.html) feature, except this time the plausible deniability isn't [broken by security researchers](https://www.researchgate.net/publication/318155607_Defeating_Plausible_Deniability_of_VeraCrypt_Hidden_Operating_Systems) and it's for any OS you want.

Due to using LUKS encryption, Tails' persistent volume feature currently offers no anti-forensics for the data in that area of your Tails stick and is therefore not airport border inspection proof. If that ever changes, we would prefer to integrate HVM more elegantly into Tails' existing infrastructure, and we appreciate the wonderful work that the Tails devs do.

### Can I install the Extension Pack in HVM's VirtualBox?

Yes. To permanently add it, edit the `env` file in your HVM dir and change the `INSTALL_EXT_PACK=` line from `"false"` to `"true"`. Then quit VirtualBox if it's open and execute the AppImage once more.

In order to run macOS in VirtualBox, you need to use the Extension Pack.

### Are HVM's virtual machines protected by Tails' Tor connection?

No, and this is actually a bonus. By having normal full-speed Internet in any VM by default, you can pretend it's a normal computer on your network but actually it's protected inside the incredibly anti-forensic environment of Tails.

You can still Torify a VM by [simply linking it to a Whonix-Gateway VM](https://whonix.org/wiki/Other_Operating_Systems). You can have the best of both worlds. But be careful, don't use a VM with clearnet Internet and then later with Torification, or vice versa, if anonymity is a concern.

### But doesn't Whonix inside Tails mean Tor-over-Tor?

With HVM's design, fortunately no. Because it connects to 'clearnet' pre-Tor Internet by default, Whonix-Gateway will connect independently of Tails' Tor process, making both able to co-exist in the one environment.

### Full Internet with DNS doesn't work in VMs by default, how do I enable it?

HVM's clearnet Internet doesn't pass DNS resolution on by default. To get normal full Internet working in a non-Torified VM, manually set the DNS servers in its system network settings to something like Cloudflare's `1.1.1.1` and `1.0.0.1`. We might be able to fix this problem in the future.

Note: This is not an issue for Whonix-Gateway, which resolve hostnames via its own Tor process inside the VM. Whonix-Workstation then points to Gateway for its DNS, as will any other Gateway-Torified VMs.

### Is HiddenVM risky software that undermines the safety of Tails?

We do indeed change a few security settings in the Tails Debian system in order to make HVM do its thing. Apart from the fact that you can inspect our code, soon we'll add to our Wiki the list of exactly what HVM temporarily modifies in your Tails environment from a security standpoint so that you can know exactly what's going on.

E.g. HVM hooks into Tails'['clearnet' user](https://tails.boum.org/contribute/design/Unsafe_Browser/#index2h2) infrastructure, which some people are already concerned about even existing in Tails.

We also increase the `sudo` timeout to improve the user experience to only need password authentication one time and because when installing HVM or during weekly updates it can sometimes take a while to do its thing. This timeout is not normally extended in Tails' Debian environment and it may make elevated privileges available to malware you can accidentally download in your Tails environment.

In the end, the factor that controls your safely more than anything else is what you choose to do in Tails. We and the Tails project can only help you so much.

As a result, we strongly suggest minimal usage of regular Tails Internet activity when also using HVM. The attack surface is already wide in Tails and HVM makes that a little wider. To do significant Tor Browser or other Internet-connected activity in the Tails host, boot into a new Tails session without launching HVM.

### Is HiddenVM a slap in the face to the whole idea of Tails?

No. HiddenVM is just an innovative and unexpected use of Tails that no one previously thought was possible.

Our project actually pays the highest compliment to Tails. We're promoting Tails as an entire new platform and ecosystem for aforensic computing, in a much wider way than before. We trust and humbly rely on Tails, Debian, Tor and Linux as upstream projects, and we feel an extreme sense of responsibility around what we're doing.

We take user privacy, security, and anonymity very seriously and will implement updates to improve the default safety for HiddenVM users over time. For now, we invite you to inspect our code and offer suggestions and contributions that improve security without removing functionality or features.

Furthermore, HiddenVM may attract more users to the Tails user base, which will enlarge its anonymity set, which is beneficial for the Tails community. And although we don't use the Tails Tor environment for our main computing and we prefer HVM Whonix instead, we are still promoting and making use of Tails as a fundamental part of the process to download and set up HiddenVM, in which it is an incredibly safe environment to do that.

As such, we are normal Tails users and advocates just like anyone else.

### Limitation of efficacy

Your data is not 'private' or 'hidden' while you are using the computer with your VeraCrypt volume unlocked. It only applies to when your computer is turned off or turned on and the private data in your VeraCrypt volume is not unlocked.

'Deniability' is very complex. There are many threat models and situations. There is no one-size-fits-all method of effective deniability. How 'normal' (i.e. 'plausible') your computer or data must convincingly appear to be, when turned off or forced to be turned on, wholly depends on your circumstances.

Our claim of effective deniability is a broad one and may not apply to your particular scenario. We may not be able to cater to your scenario but we are very interested in studying it and our Wiki could be a place to document some scenarios and solutions of deniability in the context of HiddenVM.

The Tails project lists other limitations and warnings which may apply, [please read them](https://tails.boum.org/doc/about/warning/index.en.html).

## Disclaimer

Despite our grand claims at the top of this README, any software project claiming increased security, privacy or anonymity can never provide a guarantee for such things, and we are no different here.

As our license states, we are not liable to you for any damages as a result of using our software. Similarly, any claims by our project or its representatives are personal opinions and do not constitute legal or computer security advice.

The HiddenVM project provides no guarantee of any security, privacy or anonymity as a result of you using our software. You use our software at your own risk, and whether or how you use it is your own decision.
