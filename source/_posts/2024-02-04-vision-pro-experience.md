---
title: Vision Pro Initial Experience
date: 2024-02-05 3:00:00
tags: 
 - Smart Device
categories:
 - Technology
---

I picked up the device at 8:00 AM EST on February 2nd. As someone who doesn't wear glasses, I've used it for a full two days now, and here are some of my findings.

## Configuration Choice

I chose the 512GB version, primarily because I anticipated that some of the software/games on the Vision Pro might feature more 3D scenes, which could consume more storage space. However, since I mostly use it in a Wi-Fi environment, the 1TB version seemed unnecessary.

## UI Display Quality

Having never used other VR/AR products before (Google Cardboard excluded 😂), I didn't find the Vision Pro's display quality particularly outstanding. Instead, I found that the display quality of the Vision Pro cannot fully replace that of the Studio Display. Compared to other types of displays, it has these characteristics:

**Resolution:** A 5K display has a **significantly** sharper image and noticeably better color space for the naked eye. The clarity of the Vision Pro is roughly equivalent to that of a 2.5K display, with the display quality ranking: high-end displays >> Vision Pro > high-end projectors. Upon closer inspection, I could see the pixels in the Vision Pro. My vision is not perfect, being slightly nearsighted, but I could still discern pixels without corrective lenses.

**HDR Effect:** Thanks to micro-LED, the HDR effect is relatively outstanding, better than monitors and projectors, but the brightness of highlights cannot surpass that of the iPhone's OLED or the MacBook Pro's miniLED.

**Adaptive Rendering:** The above clarity comparison was made when adjusting the window to the size I typically use on a monitor. However, the windows in the Vision Pro are adjustable, including distance and size. Moreover, the physical movement of the glasses in the real world affects the display: as you get closer, the image appears clearer. If the retina display is @2x-@3x, then Vision Pro is @1x-@9x (9x is just a figure of speech). All system UI in the Vision Pro, including SVGs/text on webpages, are vector rendered. You don’t need to zoom in; just moving closer to the display area reveals incredible clarity in every small letter/vector graphic.

**Brightness:** It doesn't seem to reach 500 nits. If you take off the glasses during the day, the real world appears significantly brighter. However, this does not greatly affect the experience.

## Passthrough Quality

The quality of the passthrough is far inferior to that of the UI display, with the camera resolution not being high, making the real world appear far from 4K quality. However, its exposure control is excellent, with no overexposure or underexposure, making it viable for daily life activities like eating. Yet, it's not suitable for reading smaller texts (too blurry to read; for example, food ingredient lists or small text on a phone are unclear).

The perspective effect is just usable. I can wear it and walk around the house without any issues.

## Comfort and Weight

Initially, wearing the Solo Band for extended periods was uncomfortable, but after switching to the Dual Loop and adjusting it carefully, I found it significantly more comfortable than at the beginning, manageable for an hour or two. The Dual Loop has two adjustment points, and I recommend experimenting with it; too tight or too loose can be uncomfortable. Lying down with it is uncomfortable as it presses against the cheeks. The weight is definitely noticeable, and I hope it can be as light as glasses one day.

## Control

Overall, the control is quite convenient. If the control of a Mac's touchpad is 10/10 and an iPad is 9/10, then the Vision Pro is already at 8.5/10 (excluding typing). Scrolling feels very responsive, with a high-refresh-rate feel, noticeably better than 60Hz. However, if you place your hand in a spot not visible to the camera (like under the table), that magical experience disappears, and you remember: ah, it's the camera capturing my gestures.

Besides pinch 🤌 controls, direct button clicks are also interactive, bringing the window "closer" and then using your index finger to tap the air screen like on an iPad, which is quite useful for websites with small buttons, such as YouTube. This approach is more convenient for websites not well adapted, where buttons are close together.

The control experience for iPad-compatible apps is also quite good. Many software (including those Apple once advertised as supporting Vision Pro) are actually just iPad-compatible versions. Trying iPad versions of Lightroom, Paramount+, Maps, etc., I found the experience surprisingly good. The biggest issue remains the typing experience. Thinking back, the front-end scheduling, mouse pointer, and Apple Pencil's Hover feature added to the iPad are amazingly similar to the interactions with Vision Pro, making iPad Apps that support these features perform well on Vision Pro.

My favorite method of typing is now using both index fingers to tap the keyboard. I've gradually gotten used to it, but it's still not as good as the iPhone's small-screen keyboard. The Bluetooth keyboard remains to be tried.

After using the Vision Pro for a while, whenever I see a screen in the real world that feels far away or improperly positioned, I instinctively want to 🤌 adjust it, only to realize I can't 😂. I think this will become a subconscious gesture in the future, similar to scenarios described in Liu Cixin's "The Three-Body Problem".

## Software Experience

The software feels like it's at a late Beta stage for Apple, not quite a final version. First, it currently only supports input in English (US), not supporting any other language input (including Chinese), both for the keyboard and Siri voice commands. Also, I noticed that visionOS 1.0.2 does not support [iMessage Contact Key Verification](https://support.apple.com/en-us/HT213465) introduced with iOS 17.2. This was hinted at by Xcode: if you set the Minimum Deployment in Xcode 15.2 to 17.2, the software won't run on visionOS 1.0.2. Thus, it can be concluded that visionOS 1.0.2 = iOS 7.1. This shows that Apple rushed the release of Vision Pro, perhaps because they felt visionOS 1.1 had too many bugs to release, leading to current functionality mismatches. For reference: iOS 17.1 was released on October 25, 2023, and iOS 17.2 on December 11, 2023.

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/57590504-ac64-4d3a-521a-4c95a977e201/extra" alt="IMG_0002 2.jpeg" width="782" height="586"/>

## External USB Devices

There is no capability for external USB devices, which is a downside compared to the iPhone and iPad. Therefore, Vision Pro does not support YubiKey 5C NFC, as it lacks both a USB port and NFC. If you want to log into a website but have only configured YubiKey, unfortunately, you can't log in. However, Passkey is supported.

## Extended Screen for Mac

Many are interested in extending the Mac screen. Personally, I find the extended screen experience far inferior to that of real monitors. For the same price, you can buy a monitor with much better display quality than Vision Pro. But if portability is your main concern, such as wanting a monitor on a plane or in a hotel, or if you value privacy, such as not wanting others to see your screen, then it has some use.

I find the display quality of Vision Pro slightly worse than that of mid-range 4K monitors (around $1000). There is also some latency, similar to the latency experienced when using an iPad as an extended screen. I would not use Vision Pro primarily for this purpose.

## Persona (Beta)

It's usable but only recreates 90-95% of a person's face. It's somewhat in the uncanny valley. Having it is better than nothing, and I think using it for video conferences wouldn't be too awkward.

## Car Passenger Experience

**Using it while on a vehicle is completely impractical**, even with Travel Mode enabled, at least in a small car. First, the Vision Pro may mislocate due to the presence of windows, sometimes thinking you are moving quickly and other times failing to locate. But this could be considered a software issue. The next problem is physical: due to its weight, your face can feel extremely uncomfortable from the acceleration during bumps. Therefore, it's not recommended during takeoff/landing/taxiing/turbulence, beware of facial pain.

Thus, the experience with an iPhone/iPad is better when riding. I will not use Vision Pro while riding in the future.

## Other Downsides

EyeSight is very limited, with low brightness and resolution. iFixit's teardown revealed the reason: compromises had to be made to replicate the 3D effect of glasses. The replication of eyes must be three-dimensional, otherwise, outsiders won't know who you are looking at.

## Conclusion

I believe Vision Pro is nearing maturity, with the software already at the usability level of a final Beta version. There are relative shortcomings in software, but I think these will be quickly addressed in the coming months.
