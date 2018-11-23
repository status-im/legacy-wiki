## Some questions

If we show the 3 words to the user during the onboarding, then what do
we do about the existing user accounts who don't get to see the initial
sign up screen any more? And if we generate it in a migration how do
they get it?

> Another way to look at it, is Console is just another contact, why
> can't he send a new message to old users saying "Hey in this update
> you now have a signing phrase, it is XYZ. The idea of Console was to
> be like your "Settings" but in a conversation interface He's like
> Windows "Paperclip" only less annoying (hopefully)

Are we going to have word lists for other languages as well? I'm
assuming this should work the same way label i18n does - use device
language if available, otherwise English. Is that correct?

> ...

Right now onboarding gives two different passphrases - one for recovery
and this one. Does it seem too much to shove all that info to the user
the moment they create their account?

> ...

What should a user do if his signing phrase gets compromised? Should he
be able to change it (generate again)?

> The signing phrase is an anti-phishing device. The idea is that if the
> user is in a fullscreen webview, the phisher has a very slim chance of
> faking it. It doesn't have anything to do with atually signing a
> transaction