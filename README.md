#Always use!

It will save you a lot of headache, no clobber = no overwrites


```
--no-clobber
```


#First we have to make a noise profile

we will do this by recording a few seconds at the beginning of the cassete.
I think this profile will work for everything recorded whit this setup.

```
sox -t coreaudio "USB Audio C" -n noiseprof ./noise.prof
```

#Then push record..

This will record the sound, through OSX coreaudio and apply the noisereduction
on the fly, the 0.21 is the fuzz so to speak. Higer values will eliminate more
shit, but 0.21, seems like a reasonable bar.

```
sox -t coreaudio "USB Audio C" ./band68.flac noisered noise.prof 0.21
```

We can add the following to the above command, to quit recording after 
eight seconds of silence.

```
silence 1 0.1 5% 1 8.0 5%
```

#Normalize

```
sox --norm input output
```

#Split by silence (5 sec)

```
sox in.wav out.wav silence 1 0.5 1% 1 5.0 1% : newfile : restart
```


#Trim any silence longer than 1 sec

```
sox in.wav out.wav silence 1 1 0.1% reverse silence 1 1 0.1% reverse
```


#Fun with sox

###Notes


> - so argument order is key when dealing with sox? 
> - Many sox options (including -t) can apply to either the input or the output. The order of the arguments controls whether it's an input or output option. 


###Make a sine-wave from 300 to 2000 Hz at 44100khz

```
sox -r 44100 -n -d synth 3 sine 300-2000
```
