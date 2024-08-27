# LONG RANGE 2
## question
Of late, whispers doth persist behind mine back. Yesterday, under the studio tower, a peculiar contraption was found by me. I am most intrigued to discover the content of their discourse.

## attempt
the attachment (https://rwctf.oss-accelerate.aliyuncs.com/LongRange2_2bf203ac61f7a3c5f24ee3a250d8abd6.zip)
was an unusually large .zip file one was a flash dump.exe which i coudnt run and started getting pop ups from windows so i left it there and focused on the wav file
i tried deepsound, strings, steghide and even spectogram all of which was useless. the audio did seem like radio with noise so i tried to reduce noice using sonic visualiser but nothing worthwile showed up.
then i tried binwalk.
![Screenshot 2024-02-01 000616](https://github.com/adwait3/tet-real/assets/148553626/bb7db896-9293-4e4f-b828-b7ea90aa6ac8)
so i researched abt this and found that mysql isam is just a way to take data froma data base and probably nothing of much improtance nut i saw that it was compressed , so i thought maybe we need to decompress it so i found a command to decompress a wav file and ran it 
"binwalk -D='.*' a.wav"
tried this and extracted this but turned out i was compressed more and more and more and it turned out to be a never ending gthing so i tried smth else 

next i thought it was high frequesncy morse code so i tried to decipher it online but couldnt get anything , i tried various things to manipulate the audio but at this point i myself didnt know what i was doing and why and then i gave up.
