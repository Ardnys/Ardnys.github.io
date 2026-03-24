+++
title = "Unfortunate Tale of quip"
date = "2026-03-24"

[taxonomies]
tags=["python", "rust", "webrtc", "sad"]

+++

# The Unfortunate Tale of quip

Around April 2025, when I was applying for Master's programmes I thought my CV was lacking on personal projects that showed my interests. Around a similar time, me and my friends were semi-regularly playing **Jackbox Games**. I found an interesting problem to solve when we had some problems with it; so it would be useful for gaming sessions, technically interesting and a funny conversation topic. I thought it would be a "fun" project to do in a weekend or two. That estimate aged very, very poorly.

This is a story about naivety, ambitions, friendship and cruelty of time.

## Workings of Jackbox Games
[Jackbox Games](https://www.jackboxgames.com/) are a collection of party games that usually require 3+ players and revolve around making jokes, drawing silly doodles and getting the most votes by being funny. For example, *[Quiplash](https://www.jackboxgames.com/games/quiplash-3)* is about making quips for prompts. In a round, 2 people get the same prompt and write their quips. After everyone enters their quip, the results are shown and everyone votes for the one they like. Votes give points and the one with most points win. It is a battle of wits and best played with close friends. 

> The project name **quip** comes from **Quip**lash, our favorite game in Jackbox Games. 

Jackbox Games work like the following: The person who owns the game (host) starts it and creates a lobby and shares the join code to friends (players). The main game is on the host's computer and players interact with the game with their own devices. If it's played IRL in the same room, there's no problem because you see the host computer and play with your phone. You can even smack your friends if they are too funny.

### Playing Jackbox Games Online

What if you want to play it online? With Discord, that's easy with application sharing. Everyone gets in a call and host shares the game with audio. It works incredibly well. But, let's say Discord has disappeared for some reason and we had to switch to alternatives. How would that work?

> Looking back from 2026, that was a scary accurate foresight. Onya 2025-Q2 me.

One of the alternatives is Google Meet. Unfortunately, Google Meet or any browser based online video conference app doesn't allow you to share application with audio. Sharing the whole screen works, but then the host has to use another device to play. As a serious, competitive Jackbox player and a host, I cannot accept this limitation. Anyways, we ended up cutting our losses and playing with alternate methods a few times but this problem gave me a funny idea.


## My funny idea
Browser is a bit strict with screen sharing, but it allows you to share **a tab with sound** so I can't help but ponder: _What if I stream an application and its audio to a browser tab and share that instead?_

Just the wicked strangeness and the slight glimmer of possibility of this idea was enough for me to at least give it a try. I mean, I dabbled with WebRTC before and I knew I could do it. The sheer silliness gave me so much adrenaline that I started researching right away.

## Le Stack
### WebRTC 
My initial dabblement with WebRTC was with Java but my little project could do with something simpler, so I found [aiortc](https://github.com/aiortc/aiortc) which is a popular WebRTC implementation in Python. It's a pretty good library that makes WebRTC somewhat easy to work with.

### Application capture
I was chuffed to bits to find [windows-capture](https://github.com/NiiightmareXD/windows-capture), a Rust library for fast window/application capture for Windows, and has Python bindings. It's a really well written library and I learned so much about Rust by studying its source code. 

After finding these two, I read the examples of `aiortc` and did some experimenting. There were webcam and local file examples, so I only had to write a custom class that captured frames with `windows-capture` instead of reading from a file or webcam. When everything seemed all clear and I pulled an all-nighter out of excitement. After more fiddling, I got the video part figured out and even wrote a humble UI with `Alpine.js` because I wanted to try it. With AV1 codec, the video looked great with low latency. I was happy, blissfully ignorant to nightmares waited ahead.

### Audio capture
Unfortunately, getting application frames to dance in the browser was the easy part as Google already figured out video-only application streaming. The lacking, possibly difficult part was **audio**. Dare I say, I was VERY lucky to have the first part go smoothly. My search efforts for **application only** audio capturing for Windows yielded either negative or too difficult. No one made a nice library like `windows-capture` for audio. There were libraries for system audio or audio devices but no application audio. 

> At this point, the lack of libraries should have been the indication that application specific audio capture **was not** the right way to solve this problem. Instead of taking a step back and thinking about alternative solutions to the problem, I considered it a challenge and a missing piece in the vast sea of libraries, and continued my pursuit full throttle like a mad horse.

To be fair, mad horse pursuit wasn't a hollow one. I already knew about virtual audio devices and I knew they could be handy for this case. The part I didn't like was their **usability**. It just doesn't feel good to install dodgy looking drivers, restart your computer, mess with the settings and hope that everything works. It introduces extra moving parts and puts all the burden on the user's shoulders. I didn't want to rely on something that would make me unhappy as a user. That was the main reason for my undue resistance to try virtual audio devices and focus too much on application audio.

Back to storytelling.

--- 

However, the mad horse was ignorant to C++ and WASAPI, and deemed them too scary. Rust, on the other hand, was familiar, approachable, intriguing. Mayhap there's a lead on the Rust part?

> Looking back at it again, although my fear of C++ and WASAPI is understandable, it may not be the right call _next time_. I have never worked directly with OS APIs, and this was a good opportunity to try and learn it. 

### [wasapi-rs](https://github.com/HEnquist/wasapi-rs)
Luckily, someone wrote a WASAPI wrapper for Rust and provided examples for application audio capturing. It wasn't nearly as nice as `windows-capture` and didn't come with Python bindings. At least it was maintained and worked. Therefore I set a new challenge: Study `windows-capture` and more-or-less take `wasapi-rs` to a similar level with Python bindings!

---

## Long Recession
### Necessary pause
My college related responsibilities caught up with me before I even started the new ambitious challenge. As adrenaline rush depleted, I paused that project and focused on my graduation project, exams, and other college related projects. This delayed the project around 2 months.

### Getting back at it in summer
I was excited to work on it after graduation. I usually only bring my Linux laptop to holiday but this time I brought an extra Windows one just to work on `wasapi-rs` bindings and quip. And I did work on it. I studied `windows-capture` and started to replicate the neat architecture to fit `wasapi-rs`. Unfortunately, it wasn't long before things started to go downhill. I got terribly sick within the first week of the holiday. That sickness drained all my energy. I couldn't do much of anything except reading and wait until I recovered.

I chipped away at `wasapi-rs` bindings and almost got it to work, but I had odd bugs in Python bindings that I could not figure out. With mood all time low, I was getting impatient and the awful climate was getting on my nerves. What should have been a little break and more elaborate debugging became an arms race of rewriting quip (the WebRTC and server part) in Rust to avoid Python bindings. At that point, I was just avoiding the problems so I paused the project again and started to work on other compelling projects, like [venv-rs](https://github.com/Ardnys/venv-rs). These projects improved my Rust skills and I enjoyed working on them a lot. 


### Getting back at it in fall
3 months later, I came back to the project after randomly deciding to read more about PyO3. This time I learned to properly debug my bindings and after a few tries I got it working. Now only the WebRTC part was left. I roughly implemented the audio bits. It still doesn't work and I don't really know why.

> "**roughly implemented** the audio bits... doesn't work". Well it was as rough as a galactic scale sandpaper that's why! But there was a bigger problem lurking in the shadows.

Only after implementing the Python bindings for `wasapi-rs` I realized that I **really** didn't think this through and gave in to the silliness of the idea. The issue is a bit complicated but try to read it slowly and visualize:

Let's say I got everything working and I stream the game to browser tab. I share the tab with audio. People hear the game. But now I, the host, hears both the game and the browser (which now plays the game audio). Now the host has messed up, nothing is actually solved by streaming the game to the browser! 

Ugh! Stop beating my dead horse! Anyways I paused the project again until further notice. It was just sad at this point. 

---

## Final Reckoning

### One last chance
I don't know why but again, 4 months later, I wanted to try it with a Virtual Audio Device (VAD) just to see what happens. 

My initial tests were promising. I still didn't like this approach but it was the only viable way I could think of. I removed the old audio code and started from scratch. But there was a different problem now: The old `Alpine.js` frontend I coded for no reason.

### Audio in browsers
Browsers know that internet is a terrible place and they actually block audio to play automatically. Users have to click something to play audio. It's a feature we all should be grateful for, but it's super annoying when you are working with WebRTC and your audio doesn't work!

### Frontend is not for me
Unfortunately my frontend was written by overly excited me in one night 10 months ago. In its current state, it was impossible to debug any audio issues because I wasn't certain if my frontend was working correctly. Even if it was, the ugliness of the UI and the code made it incredibly difficult to work on it. It was a simple UI but man `Alpine.js` is just awful to look at. I had no desire to write frontend code in the first place, but I had not even a single iota of desire to relearn `Alpine.js` again. 

> I kind of understand the appeal of libraries like `Alpine.js` but man do they end up incredibly difficult to maintain when the project gets _a little_ complicated and you leave it alone for a while. Can you believe why no one uses these for anything slightly complicated? 

Before I could fully solve the audio issues, I had to fix the depressing frontend.

### Running back to React
I opened Excalidraw and put some thought into UI design and usability. I added a "pre-share checklist" that reminds the users to install a VAD and set the correct audio settings. My design also had large thumbnails of the applications, as well as audio device playback to find the correct audio source. Actually that feature doesn't work, but it's nice that I thought about it. It also has nicer controls for the screen share. Of all the React projects I had to do, this is the only proper SPA, I think.

Thankfully I didn't have to do it alone. Claude managed to write a pretty good frontend that fit my design. I still had to fix a few things manually but it arrived in a working state pretty quickly. 

### Claude Help
Actually Claude's coding capabilities had improved significantly since the last time I tried it. It actually helped with a lot of bugs in the backend. It didn't always solve them correctly but at least it helped with the identification.

After major bugs were solved with Claude's assistance and essential features working, the project was finally in an acceptable state.

---

## The lack of elephant in the room
Pausing projects for a long time and not thinking things through is a bit sad but it's doesn't mean failure. It happens. My primary motive from the beginning was to make it work and rejoice for the fact that something like this even exists. On that end, I succeeded, albeit almost a year late. 

The biggest failure is that, my silly little project took so long that it outlived the friendships that played Quiplash. It's sad but also kind of funny, if you look at it through a vivid imagination.


## Vivid Imagination
I can't help but imagine myself finally emerging from a shed with a childish joy to an empty void: "I HAVE DONE IT! FINALLY! WE CAN PLAY QUIPLASH- where's everyone". I see the only remaining survivor in the distance sitting alone in a patch of dry grass, poking at the dirt of the wasteland with a small stick. I hastily walk towards her with a Windows laptop in my hand, streaming the main menu of Quiplash 3 from Jackbox Party Pack 7 to a browser tab. My custom UI has a soundboard and a few stickers of our favorite inner jokes, too. Like my bright green hedgehog socks, a large JPEG of Thomas' Calculus textbook, and the unmistakeable university shuttle bus that we instantly recognize a mile away. I stop when I get closer to my friend and plead for answers with almost teary eyes: "What happened here?" She turns her head slowly, but doesn't face me directly. "oh, you came. as for others, most of them went silent after graduation. the other two had a fight not long after and cut all contact. you said that you were working on a secret project and stopped responding. only i and my memories remain" said she with eyes as dead as the twig she loosely held in her hand. she wiped the red dust off of the stick and stirred a dirt soup, "at least what's left of it, anyway. even those seem to fade away in a place like this" 

I could not bring myself to say "I couldn't believe it" because it was all too real, and every sense pointed to the death and emptiness around me. I did not know what to say so I decided to lighten up the mood a bit. "At least you are here. I am, too" to which she answered colder than I expected "i guess". Oof, this is not good. Well, I could show my cool project I worked on when I was absent. Welp, there was only one option now. "Let me show you my silly project. I wanted to stream the game to browser to share it with Google Meet remember? It's finally done! I had so many unexpected and unfortunate problems but I finished it" I said, trying not to sound to insensitive. "I even put stickers and a soundboard. Remember my socks?" pointing to the small, dark 15" screen that was barely visible in the daylight of the wasteland. "You select the app here and click start. You have to click something for audio to play."

Guh, why did I give unnecessary information! She only twisted her neck a little to align her eyes to the screen and blinked 3 times. Other than that, not a single muscle moved in her face. Her response was, at best, disinterested: "i see". Unfortunately for her, I was more than a little interested so I relentlessly continued: "The visual part was easy but audio -as always- gave me so much trouble! I had to write a Python binding of a small wrapper -which I also had to write- for a Rust wrapper of WASAPI. It's wrappers all the way right? I had to rely on a virtual audio device at the end but it works! Process audio capture was not needed after all. I learned a lot about Rust so I think it's good even if I don't end up using it for anything". "rust, huh" she said as she moved her stick a bit further and stabbed it to a piece of rusted metal. "i learned about rust, too. by the time it came, nothing remained but the decay it left behind." 

Only then I realized how much time has passed since I started compiling Rust. She was right and I left.
