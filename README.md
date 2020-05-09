# hangman
# END PROJECT
#Student name: Ory Rosin

def print_hangman(num_of_tries):
    # מילון תמונות שהמפתח אליו הוא המספר נסיונות, כשנקרא למפתח נקבל את התמונה
    HANGMAN_PHOTOS={0 : '', 1 : """
     x-------x""",\
    2 : """
    x-------x
    |
    |
    |
    |
    |""",\
    3: """
    x-------x
    |       |
    |       0
    |
    |
    |
    """,\
    4 : """
    x-------x
    |       |
    |       0
    |       |
    |
    |""",\
    5 : """
    x-------x
    |       |
    |       0
    |      /|\\
    |
    |""",\
    6 : """
    x-------x
    |       |
    |       0
    |      /|\\
    |      /
    |""",\
    7 : """
    x-------x
    |       |
    |       0
    |      /|\\
    |      / \\
    |"""} 
    print(HANGMAN_PHOTOS[num_of_tries])

def show_hidden_word(secret_word, old_letters_guessed):
    blanks=''
    for i in secret_word:
        if i in old_letters_guessed:
            blanks= blanks+' '+ i
        else:
            blanks= blanks+ ' _'
    return(blanks)

def check_win(secret_word, old_letters_guessed):
    if ((show_hidden_word(secret_word, old_letters_guessed)).replace(' ','')).isalpha()== True:
        return True
    else:
        return False

def choose_word(file_path, index):
    input_file = open(file_path, "r")
    content= input_file.read()
    input_file.close()
    list_of_words= ((content.lower()).split())
    filtered_list = [] 
    for i in list_of_words: 
        if i not in filtered_list: 
            filtered_list.append(i)
    for i in range(100):
        if index > len(list_of_words):
            index= index- len(list_of_words)
        else:
            break
    return(list_of_words[index-1])

def check_valid_input(letter_guessed, old_letters_guessed):
    if len(letter_guessed)>1 or letter_guessed.isalpha()== False or old_letters_guessed.count(letter_guessed.lower())>= 1 :
        print('X')
        return False
    else:
        old_letters_guessed= old_letters_guessed.append(letter_guessed.lower())
        return True

def try_update_letter_guessed(letter_guessed, old_letters_guessed):
    if check_valid_input(letter_guessed, old_letters_guessed)== False:  
        print(' -> '.join(old_letters_guessed))
        return False
    else:
        return True

def opening_page():
    print("""               _    _                                         
              | |  | |                                        
              | |__| | __ _ _ __   __ _ _ __ ___   __ _ _ __  
              |  __  |/ _` | '_ \ / _` | '_ ` _ \ / _` | '_ \ 
              | |  | | (_| | | | | (_| | | | | | | (_| | | | |
              |_|  |_|\__,_|_| |_|\__, |_| |_| |_|\__,_|_| |_|
                       __/ |                      
                      |____/ """)
    print('WELCOME TO HANGMAN GAME. YOU WILL HAVE 6 TRIES TO GUESS A WORD. GOODLUCK!!!') 

def main():
    num_of_tries, max_tries= 1,6
    old_letters_guessed=[]
    opening_page()
    file_path= input('Enter a path of words file: ')
    index=int(input('Enter a number represents place of word to guess: '))
    print("Let's start!")
    print_hangman(1)
    secret_word=choose_word(file_path, index)
    print(len(secret_word)* ' _')
    while num_of_tries <= max_tries:
        letter_guessed=input('Guess a letter: ')
        if check_valid_input(letter_guessed, old_letters_guessed)== True:
            if letter_guessed in secret_word:
               print(show_hidden_word(secret_word, old_letters_guessed))
               if check_win(secret_word, old_letters_guessed)== True:
                   print('WIN')
                   break
            else:
                print(':(')
                num_of_tries= num_of_tries+1
                print_hangman(num_of_tries)
                print(show_hidden_word(secret_word, old_letters_guessed))
                if num_of_tries==7:
                    print('LOSE') 

if __name__ == "__main__":
        main()

