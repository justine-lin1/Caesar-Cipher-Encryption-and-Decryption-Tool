"""
Justine Lin
May 5th
"""

import string


def read_word_list(file_name):
    '''
    file_name (str): the name of the file containing the list of words
    to load.

    Returns: a set of valid words. Words are strings of lowercase letters.

    Depending on the size of the word list, this function may
    take a while to finish.
    '''
    in_file = open(file_name, 'r')  # in_file: file
    line = in_file.readline()  # line: str
    word_list = line.split()  # word_list: list of str
    in_file.close()
    return frozenset(word_list)  # creates an immutable set.


def read_message_string(file_name):
    """
    Returns: a possibly profound message in encrypted text.
    """
    in_file = open(file_name, "r")
    message = str(in_file.read())
    in_file.close()
    return message


def create_shift_table(shift):
    '''
    Creates a dictionary that can be used to apply a cipher to a letter.
    The dictionary maps every uppercase and lowercase letter to a
    character shifted down the alphabet by the input shift. The dictionary
    should have 52 keys of all the uppercase letters and all the lowercase
    letters only.

    shift (int): the amount by which to shift every letter of the
    alphabet, 0 <= shift < N_LETTERS. If shift is less than zero or
    greater than or equal to N_LETTERS, this function should return
    None.

    Returns: a dictionary mapping a letter (str) to another
             letter (str), for all lower and upper case letters.
    '''
    import string
    ls = {}  # will be the dictionary
    if shift < 0 or not shift < N_LETTERS:
        return None
    else:
        letters = string.ascii_lowercase + string.ascii_uppercase
        # "table" - will find index positions

        # Creating the dictionary that will be used as "table"

        for i in letters:  # Check if going too far in shift, check upper or lower
            if i.islower():
                s = (letters.index(str(i)) + shift) % 52  # Shifted index. remainder is the index
                s = letters[s]  # Shifted character
                if s.isupper():
                    s = s.lower()  # Convert back to lower case, as initial letter is lower
                ls[i] = s
            if i.isupper():
                q = (letters.index(str(i)) + shift)  # Shifted index
                if q >= 52:
                    q = q % 52  # Remainder is the index of the letter
                    q = letters[q].upper()  # Need to uppercase in case shifted letter is lower
                else:
                    q = letters[q]  # q will now be the letter in the given index position of letters
                ls[i] = q  # Adding q to the dictionary. q is now the shifted letter of i
    return (ls)


def apply_shift(original_text, shift):
    '''
    Applies the Caesar Cipher to original_text with the input shift.
    Creates a new string that is original_text shifted down the
    alphabet by some number of characters determined by the input shift
    shift (int): the shift with which to encrypt the message.
    0 <= shift < N_LETTERS

    Returns: the message text (str) in which every character is shifted
             down the alphabet by the input shift
    '''
    ls = create_shift_table(shift)  # Need to use previous function to get shifted table
    shiftstring = ""  # Will be the shifted original_text
    for i in original_text:
        if not i.isalpha():  # In case there is punctuation or a space, add to shifted original_text
            shiftstring += i
        else:
            shiftstring += ls[i]  # Adding shifted letter to the new string
    return (shiftstring)


def encrypt_message(original_text, shift):
    '''
    Used to encrypt the message using the given shift value.

    Returns: The encrypted string.
    '''
    return apply_shift(original_text, shift)


def is_word(word):
    '''
    Determines if word is a valid word, ignoring
    capitalization and punctuation

    word (str): a possible word.

    Returns: True if word is in word_list, False otherwise

    Example:
    >>> is_word('bat')
    True
    >>> is_word('asdf')
    False
    '''
    garbage = " !@#$%^&*()-_+={}[]|\:;'<>?,./\""
    return word.strip(garbage).lower() in VALID_WORDS


def decrypt_message(cipher_text):
    '''
    Decrypt cipher_text by trying every possible shift value
    and find the "best" one. "Best" will be defined as the shift that
    creates the maximum number of real words when we use apply_shift(shift)
    on the text. If s is the original shift value used to encrypt
    the message, then we would expect N_LETTERS - s to be the best shift value
    for decrypting it.

    Note: if multiple shifts are equally good such that they all create
    the maximum number of words, you may choose any of those shifts (and
    their corresponding decrypted messages) to return

    Returns: a tuple of the best shift value used to decrypt the message
    and the decrypted message text using that shift value.
    '''
    best_shift = 0
    text = cipher_text.split()
    best_yet = 0
    all_msgs = []
    decrypt_msg=''
    for i in range(26):  # Testing all the letters in the alphabet
        correct_words = 0  # Resetting correct_words for each letter
        i_list = []
        shift = i
        ls = create_shift_table(shift)
        for w in text:
            word = ''
            for x in w:  # To access each letter in the word
                if not x.isalpha():
                    word = word + x
                else:
                    word = word + ls[x] # Giving x the value of the shifted table
            i_list.append(word)
            if is_word(word):
                correct_words += 1 #Updating the number of words
        all_msgs.append(i_list)
        if correct_words >= best_yet:  # Seeing how it compares to previous shifts
            best_yet = correct_words  # Setting a new "best_yet"
            best_shift = i
    count=1
    for wrd in all_msgs[best_shift]:
        if count < len(text): # So that spaces are added between all words except for the last
            decrypt_msg = decrypt_msg + wrd + ' ' # To add spaces between words to make sentence
            count += 1
        else:
            decrypt_msg = decrypt_msg + wrd # To not add space at the end of sentence
    return (best_shift, decrypt_msg)



N_LETTERS = len(string.ascii_lowercase)
VALID_WORDS = read_word_list('words.txt')

# Unit test - verify that create_shift_table is properly implemented.

message = "Hello, World!"
print("*** create_shift_table:", end=" ")
assert create_shift_table(-1) == None
assert create_shift_table(N_LETTERS) == None

n_errors = 0
for i in range(N_LETTERS):
    table = create_shift_table(i)
    assert len(table) == N_LETTERS * 2
    for key in table:
        alt = table[key]
        if key.isupper():
            base = ord('A')
        elif key.islower():
            base = ord('a')
        org = chr(((ord(key) - base) + i) % N_LETTERS + base)
        if alt != org:
            n_errors += 1

if n_errors != 0:
    print("FAILED")
else:
    print("PASSED")
assert n_errors == 0

# Unit test - verify that apply_shift is properly implemented.

print("*** apply_shift:", end=' ')
result = apply_shift(message, 1)
if result != 'Ifmmp, Xpsme!':
    print("FAILED")
else:
    print("PASSED")

# Test encryption.

plaintext = 'Python is pretty useful.'
expected = 'Ravjqp ku rtgvva wughwn.'
print('Expected Output:', expected, end=' ')
print('Actual Output:', encrypt_message(plaintext, 2))
assert expected == encrypt_message(plaintext, 2)

# Test decryption.

ciphertext = "Kdssb Vxpphu, L krsh!"
expected = (23, 'Happy Summer, I hope!')
print('Expected Output:', expected, end=' ')
print('Actual Output:', decrypt_message(ciphertext))
assert expected == decrypt_message(ciphertext)

# Actually try to decode the message.

ciphertext = read_message_string("message.txt")
result = decrypt_message(ciphertext)
print("Best shift:", result[0])
print(result[1])
