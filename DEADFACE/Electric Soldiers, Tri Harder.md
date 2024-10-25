# chall
`We stumbled across this image from d34th that might indicate how DEADFACE plans to sneak stolen information through various networks without detection. According to Ghost Town, d34th has been refining his process for embedding hidden information in various files.

See if you can uncover any hidden information in this image. Submit the flag as flag{flag-text}.`

this challenge basically involved a png image which had a mp3 file which needed to be extraced and the flag was in the lsb of the mp3 file

* first we see that the png in hex ed has ID# in the end which indicated our mpr , we extract this audio file
* `dd if=electricsoldiers.png of=outfile bs=1 skip=1700017`
* afterwards we use
```
def extract_flag_from_mp3(stego_file, flag_length):
    # Read the stego PNG file
    with open(stego_file, 'rb') as f:
        stego_data = f.read()

    # Find the MP3 data start position (end of PNG data)
    png_signature = b'\x89PNG\r\n\x1a\n'
    png_end = stego_data.find(b'IEND') + 8  # 'IEND' chunk + 4 bytes CRC

    # Extract the MP3 data
    mp3_data = stego_data[png_end:]

    # Extract the flag bits from the LSB of the MP3 data
    flag_bits = ''
    for byte in mp3_data:
        flag_bits += str(byte & 1)

    # Convert binary to ASCII
    flag_chars = [chr(int(flag_bits[i:i+8], 2)) for i in range(0, len(flag_bits), 8)]
    flag = ''.join(flag_chars)

    # Truncate to the actual flag length
    flag = flag[:flag_length]

    return flag

# Input file
stego_file = 'electricsoldiers.png'
flag_length = 100

# Extract the flag
extracted_flag = extract_flag_from_mp3(stego_file, flag_length)
print(f"Extracted Flag: {extracted_flag}")
```

* our script to get the lsb data from the mp3 which gives the flag


------------------------------------------

# tri Harder
`The security team at NexGen Softworks reached out to us regarding a potential tip about DEADFACE. They were alerted to unusual email traffic between one of their employees and a third-party vendor regarding infrastructure upgrades. The email chain looks fairly innocent, but NexGen’s SEIM flagged it as suspicious and they’re not sure why.`

this challeneg basically had an email in which Instead of using letters or numbers, the secret message is encoded in the whitespace between the words.

## three types
Ternary Code: The whitespace is not just ordinary spaces. It's actually using three different types of spaces: a normal space, a tab, and a newline. These three types represent the digits 0, 1, and 2 in a base-3 number system (ternary).

Binary Conversion: The ternary numbers are converted into binary numbers. Binary uses only two digits, 0 and 1.

Plaintext: Finally, the binary numbers are converted into letters and symbols to reveal the secret message.

* in th email we find a large whhitespace
* checking it we find U+200A, U+2002, and U+2009 unicode characters in there.
* we convert unicode to ternary ie, determine the right values and replace them with 0, 1, and 2.
* we convert ternary to binary using
```
import sys

def ternary_to_binary(ternary_str):
    # Convert the ternary string to a decimal (base-10) integer
    decimal_value = int(ternary_str, 3)

    # Convert the decimal value to a binary string and remove the '0b' prefix
    binary_str = bin(decimal_value)[2:]

    return binary_str

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python script_name.py <ternary_value>")
        sys.exit(1)

    ternary_value = sys.argv[1]
    try:
        binary_value = ternary_to_binary(ternary_value)
        print(f"Ternary: {ternary_value} -> Binary: {binary_value}")
    except ValueError:
        print("Invalid ternary value. Please provide a valid ternary number.")
```

* we convert binary to readable and get the flag. 
