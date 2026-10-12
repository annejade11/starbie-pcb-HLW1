# Cheesie PCB

In this project, I used the pre-existing Starbie tutorial repo by SharKingStudios to make my own "Cheesie" PCB build for week one of the Half Life challenge.

Before this project, I had never worked with PCBs or physical hardware before, but it's something I've been meaning to get into for a long time as I started coding a bit over a year ago. Ever since I started coding and 3D printing things on printers at my school, I just thought it would be so cool to be able to code my own product, then put it together in hardware and print a beautiful design for it. The concept of being my own little "factory" is so cool to me and this first week of Half Life has been the perfect opportunity for me to start learning something I just haven't made the time for before. 

So let's start with the basics.

What is Starbie?

Starbie is a small pcb with a 128x64 pixel OLED screen that displays a sprite of your choice. It has different activities, like napping or playing, and also has some mood bars for energy, joyfulness, and fullness. The tutorial repo describes it as a kind of "desktop tomadachi" and I honestly think that's a perfect description. It uses a DHT11 to detect temperature and humidity as well as an MPU6050 to detect movement, so it senses when you shake it and gives you a digital reaction. It's super cute and the tutorial makes it very beginner friendly.

My Process

Part 1: Hardware

The first part of building Starbie is designing it. I chose to use KiCad, as that's what's recommended by Half Life and also what the tutorial repo uses. I followed the instructions to design the schematic, which is basically all the components you're going to need labeled with what each output/input is recieving/sending.

Here's what mine looked like:
<img width="707" height="753" alt="Screenshot 2026-10-09 140539" src="https://github.com/user-attachments/assets/9d754e10-65c5-4e57-a34e-bb6fe02c9e61" />

The next step in PCB design is routing, which means visually connecting everything so that you know where all the soldering and stuff is going to go. Because the lines can't cross over each other otherwise signals will mix, this step is super important because you're making sure everything can be connected. In my experience with Starbie, I actually had to change the order of ports (as well as the rotation and positioning of components) multiple times because I would end up in a situation where a connection couldn't get from A to B without crossing over a bunch of other connections. I also learned that the board has two sides, so some connections which just couldn't all fit on the front went on the back side of the PCB. I added some cute stickers to the PCB too (as well as some holes for mounting, but this is fully optional I just figured I might want to screw my PCB into a case). 

Here's the final result:
<img width="732" height="604" alt="Screenshot 2026-10-09 140454" src="https://github.com/user-attachments/assets/1405d63a-bc4a-467e-9249-6c0498069717" />
The red connections are the front, while that one thick blue line is what I have on the back of the PCB.

KiCad also has a really cool feature where you can see a 3D rendering of your PCB once you've routed it.

<img width="663" height="438" alt="Screenshot 2026-10-09 223958" src="https://github.com/user-attachments/assets/29d1d955-7954-4e8f-954b-4c2e71e46b4d" />

Seeing a (kind of) physical rendering of my work was super gratifying.

Part 2: Software

The next part of building Starbie was to do the code. The tutorial repo actually gives you a set of code already in case you don't know how, but I wanted to do some of it myself. Unfortunately, I didn't have the time to really dig into it as much as I might have liked, but I still wanted to customize it. (Note: I did have to edit the ports as I ended up moving them around in a different placement than the tutorial did, and anyone doing this should verify they have the same ports as what's in the code!)

The first thing I did was make my own sprite. I used a web-based sprite generator (not AI, just a platform that lets you draw pixelated sprites) called piskel. I made a fun little Peacock who remains unnamed, but I think she turned out very cute! For now I'll just go with Princess because the word "Peacock" contains the word "Pea" which reminds me of the Princess and the Pea, but I promise I'll choose something better later. 

<img width="314" height="317" alt="Screenshot 2026-10-10 202639" src="https://github.com/user-attachments/assets/29e75b24-846f-471f-b19c-2f41a0b7be0c" />

Then, because you can't just drag and drop an image into code and say "draw this", I used a link from the tutorial to turn the photo into code. Here's what that looked like when it was done:

<img width="759" height="193" alt="Screenshot 2026-10-11 195814" src="https://github.com/user-attachments/assets/0034f0e3-216c-4e7b-8c12-cb814c004649" />

While I can't say I understand how every number works, my impression is that it's instructions that tell the code if each pixel on a 32 x 32 pixel grid is black or white. The screen I'm using can only display one color, which is white, (plus black when it's off), so the code shouldn't need to specify the exact shade of blue or gray for each pixel.

Note: Much of the following section is explaining functions and if statements for non-coders. If you already know this, you can skip to the image in which I create a function "drawBubbles" to display bubbles whenever the pet is in "PLAY" mode.

The other thing I wanted to do to customize my code was a small effect. The tutorial walks you through adding a sparkle effect, and I decided this was something I could easily do myself without needing to understand all the code. The example essentially created a new custom function that drew a sparkle. Then, it set that function to be called repeatedly at specific time intervals while the pet was in "nap" mode. For people who aren't coders, a function is basically a set of instructions. If you don't show it to anyone, nothing gets done and its as if it didn't exist in the first place. This is why you need to "call" the function. Calling a function is when you reference these instructions somewhere else in your code. For example, imagine you told a friend, "whenever you open the door, follow protocol 5". Without knowing what protocol 5 is, your friend is confused and does not know what to do. But if you tell your friend, "whenever you open the door, follow protocol 5" and then give your friend a seperate sheet of paper which reads, "Protocol 5: first, close the door, then take your key, then lock the door", your friend now knows what to do. Protocol 5 has basically become an abbreviation, or a faster way to reference the instructions without needing to say them each time. That's what functions do to the computer. You give it a condition, the computer version of "whenever you open the door", and then give it the instructions to do when that statement is true. So getting back to the focus, I made a new custom function / protocol called drawBubbles. I then set it to activate whenever the pet was in "PLAY" mode so you get a cute little bubble at a random spot on the screen. I designed the bubble myself (again on piskel) and then gave the instructions to the computer on how to draw it. This time, I did it manually, with a function called drawPixel. You give drawPixel the x and y coordinate of your pixel on the Cartesian plane (think of the graphs from your math classes but 0,0 is the top left corner of your screen) as well as the color of the pixel, and it'll draw it. I set this a little more fancily (is that a word?) and used a placeholder for my x and y coordinates creatively named "x" and "y". Whenever the function is called, the sentence which tells the computer to run the instructions will also have to include the x it wants to use and the y it wants to use. Going back to the door example, this is as if you set the "door" in protocol 5 to be a placeholder. Then, you tell your friend, "whenever you open the door, follow protocol 5 specifically for the door you just opened". This allows you to write one protocol and it'll work for when your friend leaves through the front door, the back door, or even the garage door. They won't open the front door and then go around to close and lock the back door, because you told your friend to follow the protocol for the door they just opened. I used this system to write my code for the bubble, so when I type things like "x+2" it means "whatever x coordinate you're using, but add 2". I treat the placeholders "x" and "y" as my (0,0) and build everything else relative to that—one to the right of 0,0, one to the right and one down from 0,0, etc.

<img width="369" height="308" alt="Screenshot 2026-10-11 195756" src="https://github.com/user-attachments/assets/e813436f-f7f3-48ac-bc12-ec84319d1c20" />

Also what the bubble looks like for anyone who was curious (the red dot represents 0,0 or the origin):
<img width="160" height="167" alt="Screenshot 2026-10-11 152006" src="https://github.com/user-attachments/assets/013723da-4a9b-42c8-ba7a-f9a063a96f46" />

Yay! The code is finally done, and so is the hardware design. Starbie accomplished!

But WAIT! There's more!

Bonus Part: Designing the case

I wanted to add more to this PCB, but I'd reached the end of my expertise code and hardware wise. So instead, I went with something I'm a bit better at: CAD, or computer aided design. This is digitally designing or creating a 3D object which can then be manufactured, for example by printing your model with a 3D printer. I decided to use these (still slightly limited) CAD skills of mine for good and make a case for my PCB. Originally, the theme for the PCB was supposed to be planet themed, so I wanted to make a planet keychain. I spent some time on that, and honeslty, it looked okay. Here's a glimpse:

<img width="486" height="525" alt="Screenshot 2026-10-10 230121" src="https://github.com/user-attachments/assets/a362e1ff-dc0d-4b87-8216-ee92ffb4f0e1" />

However, this planet had a lot of issues. I designed it by hollowing out the inside for the PCB, but because of the curve on the planet and the fact that the screen is... well... decidedly less curved, I couldn't push the display up so that it was flush with the outside of the planet since the display would be attached to the rest of the PCB which would not fit on any of the faces of the planet. This was not ideal, as it meant I had to open the keychain just to see inside it. I decided I did not like this one bit, so it was time for a redesign. I instead made a cheese keychain (another thing I'm passionate about—look, cheese is really good) and because it was a block of cheese I could use the flat sides to make cutouts for the screen and buttons. Here's the final product, with the blue piece being the front and the gray piece being the back:

<img width="711" height="676" alt="Screenshot 2026-10-11 172724" src="https://github.com/user-attachments/assets/e937ccbd-af81-412f-94cd-4e718b2c5bcd" />
<img width="588" height="591" alt="Screenshot 2026-10-11 164717" src="https://github.com/user-attachments/assets/a52025ab-49cb-4f4a-baba-0b9ebd8be91b" />

View of the inside of the blue piece where space for the PCB and usb cable were cut out:
<img width="690" height="703" alt="Screenshot 2026-10-11 163510" src="https://github.com/user-attachments/assets/587c1176-6728-4b48-8c79-9041253fbbed" />

And yes, this is where the name "Cheesie" originated from. It's kind of corny, I know, but I still think it's cute. The holes in the back piece also allow for better air circulation which is good because the pcb needs to be able to read the humidity and tempeerature of the air around it (remember how I mentioned the MPU 6050 does that?).

So that's the PCB. Design, code, and case, all ready to go.

Softwares:
Here are all the programs I used as well as their respective purposes.
PCB design & routing - KiCad
Sprite/Pixel art making - Piskel (web-based)
Code - Arduino IDE
Case CAD - Onshape (web-based)

All these are free to use for students (onshape may require you to pay if you're not a student but I'm not sure).

From the digital to the physical:

The next step is obviously to build the PCB. I'm not doing that until week 5 of the Half Life project (if my project gets approved fingers crossed) so I'll have to get back to you on the actual building part. Still, there's a digital aspect to the building, and that's called the BOM, or the Bill of Materials. In case you didn't get it from the title, the BOM is just a list of all the parts you're going to need, how much they are, and where you're getting them from. Here's mine:

<img width="878" height="629" alt="Screenshot 2026-10-11 203200" src="https://github.com/user-attachments/assets/a1fe75a2-258e-4fd6-8b5c-5529c5adc24b" />

A couple things to note about my BOM:
1) THIS DOES NOT INCLUDE TOOLS. This is just a list of any parts I'd need, there are additional required tools that aren't on here.
2) I have two items on there listed as $0.01. No, I did not find a really awesome deal on key caps and key switches, I actually have some at home. I bought key switches for my 3D printing and I plan on printing some key caps so I don't have to buy them. However, I've left them on the bill of materials because even though I'm not buying them they are still necessary for my project.

Even though it's been a bit stressful trying to finish this on time, I am super grateful for Half Life for giving me tutorials and making it so easy to start learning something interesting. We take the path of least resistance, after all, and Half Life has really made sure there's very little resistance there. I've had lots of fun learning and I hope I'll get to keep building on it. I'm incredibly excited to build this for real in a couple weeks and also super proud of myself for finishing it considering I started on day 5 out of 7 because of school work and wasn't sure I'd have the determination to do it all. Good luck to anyone attempting the same thing and congratulations to all those who've also built starbies of their own this week!
