
+++
date = "2013-10-12 17:01:00+00:00"
draft = false
tags = ["python", "tamil", "palindrome"]
title = "Check Tamil word or sentence is palindrome"
url = "/post/63834696015/check-tamil-word-or-sentence-is-palindrome"
+++
How to check given text is palindrome or not

    def sanitize(text):
        for char in [" ", ".", ",", ";", "\n"]:
            text = text.replace(char, "")
        return text
    
    def palindrome(word):
        # This approach is o(n), problem can be solved with o(n/2)
        # I am using this approach for brevity
        return word == word[::-1]
    
    palindrome(sanitize("madam")) # True
    palindrome(sanitize(u"விகடகவி")) # False

Here is the hand made version for Tamil

    # Assign the variable meal the value 44.50 on line 3!
    # hex values from க..வ
    def sanitize(text):
        for char in [" ", ".", ",", ";", "\n"]:
            text = text.replace(char, "")
        return text
    
    dependent_vowel_range = range(0xbbe, 0xbce)
    
    def palindrome_tamil(text):
        front, rear = 0, len(text) - 1
        while True:
            #We will start checking from both ends
            #If code reached centre exit
            if front == rear or abs(front - rear) == 1:
                return True
            else:
                if ord(text[front+1]) in dependent_vowel_range and ord(text[rear]) in dependent_vowel_range:
                    if text[front] == text[rear-1] and text[front+1] == text[rear]:
                        front += 2
                        rear -= 2
                    else:
                        return False
                else:
                    if text[front] == text[rear]:
                        front += 1
                        rear -= 1
                    else:
                        return False
    
    
    
    
    print palindrome_tamil(sanitize(u"விகடகவி")) == True
    text = u"""
    யாமாமாநீ யாமாமா யாழீகாமா காணாகா
    காணாகாமா காழீயா மாமாயாநீ மாமாயா
    """
    print palindrome_tamil(sanitize(text)) == True
    
    #output
    True
    True
