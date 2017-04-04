# Google Applied CS with Android

In this Google Workshop we implement concepts from Data Structures, Algorithms, and Artificial Intelligence in order to solidify and apply concepts from algorithms and data structures of CS coursework.

Program Overview: https://cswithandroid.withgoogle.com


##### Applications:

[1) A-1-Anagrams](#A-1)

[2) A-2-Scarne's Dice](#A-2)

[3) A-3-](#A-3)

[4) A-4-](#A-4)

[5) A-5-](#A-5)



## Program Descriptions:


---


<a name="A-1"></a>
### A-1-Anagrams

For this workshop, you will be creating an Android app for a simple anagram game.

An anagram is a word formed by rearranging the letters of another word. For example, cinema is an anagram of iceman.

The mechanics of the game are as follows:

* The game provides the user with a word from the dictionary.
* The user tries to create as many words as possible that contain all the letters of the given word plus one additional letter. Note that adding the extra letter at the beginning or the end without reordering the other letters is not valid. For example, if the game picks the word 'ore' as a starter, the user might guess 'rose' or 'zero' but not 'sore'.
* The user can give up and see the words that they did not guess.

In order to ensure that the game is not too difficult, the computer will only propose words that have at least 5 possible valid anagrams.


**Tour of the code**

The starter code is composed of two java classes:

* AnagramsActivity: In Android development an Activity is a single, focused thing that the user can do. Most of our apps in this class will have a single activity but often apps are made up of multiple activities (e.g. login, settings, etc.). The starter code implements several methods:
* onCreate: this method gets called by the system when the app is launched. It is made up of some boilerplate code plus code that open the word list to initialize the dictionary and code to connect the text box to the processWord helper.
* processWord: a helper that adds words to the UI and colors them
* onCreateOptionsMenu: boilerplate
* onOptionsItemSelected: boilerplate
* defaultAction: this is the handler that is called when the floating button is clicked. Depending on the game mode, it either starts the game or shows the missing answer to the previous game.
* AnagramDictionary: This class will store the valid words from the text file and handle selecting and checking words for the game. This is where your code will among the following methods:
* AnagramDictionary: The constructor. It should store the words in the appropriate data structures (details below).
* isGoodWord: Asserts that the given word is in the dictionary and isn't formed by adding a letter to the start or end of the base word.
getAnagrams: Creates a list of all possible anagrams of a given word.
* getAnagramsWithOneMoreLetter: Creates a list of all possible words that can be formed by adding one letter to the given word.
* pickGoodStarterWord: Randomly selects a word with at least the desired number of anagrams.
Implementation

We will start by implementing a simplified version of the game that has the user guess anagrams of the given word.

To do so, your first task will be to advance the implementation of the AnagramDictionary's constructor. Each word that is read from the dictionary file should be stored in an ArrayList (called wordList).

We will store duplicates of our words in some other convenient data structures later but wordList will do for now.

**getAnagrams**

Implement getAnagrams which takes a string and finds all the anagrams of that string in our input. Our strategy for now will be straight-forward: just compare each string in wordList to the input word to determine if they are anagrams. But how shall we do that?

There are different strategies that you could employ to determine whether two strings are anagrams of each other (like counting the number of occurences of each letter) but for our purpose you will create a helper function (call it sortLetters) that takes a String and returns another String with the same letters in alphabetical order (e.g. "post" -> "opst"). Determining whether two strings are anagrams is then a simple matter of checking that they are the same length (for the sake of speed) and checking that the sorted versions of their letters are equal.

At this point, you should have a working app so try running it on your device and verify that it works. You can change the hard-coded return value of pickGoodStarterWord to try out your code with different words (e.g. "skate").

Forming anagrams with an added letter

Unfortunately, the straight-forward strategy will be too slow for us to implement the rest of this game. So we will need to revisit our constructor and find some data structures that store the words in ways that are convenient for our purposes. We will create two new data structures (in addition to wordList):

A HashSet (called wordSet) that will allow us to rapidly (in O(1)) verify whether a word is valid.
A HashMap (called lettersToWord) that will allow us to group anagrams together. We will do this by using the sortLetters version of a string as the key and storing an ArrayList of the words that correspond to that key as our value. For example, we may have an entry of the form: key: "opst" value: ["post", "spot", "pots", "tops", ...].
As you process the input words, call sortLetters on each of them then check whether lettersToWord already contains an entry for that key. If it does, add the current word to ArrayList at that key. Otherwise, create a new ArrayList, add the word to it and store in the HashMap with the corresponding key.

**isGoodWord**

Your next task is to implement isGoodWord which checks:

the provided word is a valid dictionary word (i.e., in wordSet), and
the word does not contain the base word as a substring.
For example, with the base word 'post':

Input                      | Output
---------------------------| ------
isGoodWord("nonstop")      | true
isGoodWord("poster")       | false
isGoodWord("lamp post")    | false
isGoodWord("spots")        | true
isGoodWord("apostrophe")   | false
Checking whether a word is a valid dictionary word can be accomplished by looking at wordSet to see if it contains the word. Checking that the word does not contain the base word as a substring is left as a challenge!

**getAnagramsWithOneMoreLetter**

Finally, implement getAnagramsWithOneMoreLetter which takes a string and finds all anagrams that can be formed by adding one letter to that word.

Be sure to instantiate a new ArrayList as your return value then check the given word + each letter of the alphabet one by one against the entries in lettersToWord.

Also update defaultAction in AnagramActivity to invoke getAnagramsWithOneMoreLetter instead of getAnagrams.

At this point, you should have a working app so try running your app on your device and verify that it works although the game will be a bit boring since it will always use the same starter word.

**pickGoodStarterWord**

If your game is working, proceed to implement pickGoodStarterWord to make the game more interesting. Pick a random starting point in the wordList array and check each word in the array until you find one that has at least MIN_NUM_ANAGRAMS anagrams. Be sure to handle wrapping around to the start of the array if needed. Run your app again to make sure it's working.

**Refactoring**

At this point, the game is functional but can be quite hard to play if you start off with a long base word. To avoid this, let's refactor AnagramDictionary to give words of increasing length.

This refactor starts in the constructor where, in addition to populating wordList, you should also store each word in a HashMap (let's call it sizeToWords) that maps word length to an ArrayList of all words of that length. This means, for example, you should be able to get all four-letter words in the dictionary by calling sizeToWords.get(4).

You should also create a new member variable called wordLength and default it to DEFAULT_WORD_LENGTH. Then in pickGoodStarterWord, restrict your search to the words of length wordLength, and once you're done, increment wordLength (unless it's already at MAX_WORD_LENGTH) so that the next invocation will return a larger word.


---


<a name="A-2"></a>
### A-2-Scarne's Dice

In this workshop, we will create a game from scratch in Android Studio. The game that we will be creating is a dice game called Scarne's dice (you may already be familiar with it from a previous unit).

**Scarne's dice**

Scarne’s Dice is a turn-based dice game where players score points by rolling a die and then:

* if they roll a 1, score no points and lose their turn

* if they roll a 2 to 6:

  * add the rolled value to their points

  * choose to either reroll or keep their score and end their turn

The winner is the first player that reaches (or exceeds) 100 points.

For example, if a player starts their turn and rolls a 6, they can choose to either ‘hold’ and end their turn, in which case they can add the 6 to their score, or to reroll and potentially score more points.

Let’s say they decide to roll again, and they get a 4. They now have the option to end their turn and add 10 points (6 + 4) to their score, or to roll again to get even more points.

They decide to roll again, but get a 1. Getting a 1 makes the player lose all the points from their turn (so their score is the same as before their turn), and finishes their turn, allowing the second player to begin their turn.

This goes on until one of the players reaches 100 points or more.

**Implementing the UI**

As mentioned in the preparation for this workshop, there is no starter code for this activity but we do provide you with some some images for the dice faces.

If you finished creating the UI in the preparation activity, you can skip this next step. Start by creating a blank activity and create the UI shown in the image below using either the visual editor or the XML editor (or probably a combination of both). The UI is composed of:

* A TextView to display the score and status of the game
* An ImageView to display the current die (default to the image of your choice)
* Three buttons to either roll the die, end your turn or start over


**Implementing the game**

All the game logic for this app will be implemented in the Activity class (the file will be called MainActivity.java if you accepted the default name). The Activity template has some default methods to which you will add:

* Four global variables to store:
  * the user's overall score state
  * the user's turn score
  * the computer's overall score
  * the computer's turn score
* A click handler for the "Roll" button that will:
  * randomly select a dice value
  * update the display to reflect the rolled value

Use getResources().getDrawable in order to programmatically access other images. This functionality will also be needed for the computer turn so a helper function to roll the die may be useful to implement.

Then start creating game logic. If the roll is not a 1, update the user's turn score by the value of the roll and update the label to "Your score: 0 computer score: 0 your turn score: X". If the roll is a 1, reset the turn score to 0 and update the label accordingly. TextView can be edited programmatically by calling findViewById to get the TextView object.

Having written the basic "Roll" functionality, you can tackle the other two button handlers:

* When ResetButton is clicked, reset the 4 global variables to 0 and update the label text
* When HoldButton is clicked, updating the user's overall score, reset the user round score and update the label.

At this stage, the basic user turn functionality is in place. Now, you can implement the computer turn. Start off with a very simple strategy for the computer: if the computer's round score is less than 20, re-roll, otherwise hold.

Create a helper method called computerTurn, it will need to:

* Disable the roll and hold buttons
* Create a while loop that loops over each of the computer's turn. During each iteration of the loop:
  * pick a random die value and display it (hopefully using the helper you created earlier)
  * follow the game rules depending on the value of the roll.

Be sure to update the label with the computer's round score or "Computer holds" or "Computer rolled a one" as appropriate.

Finally, invoke the computerTurn procedure from the both the HoldButton handler and the RollButton handler (if the user rolled a 1).

You may find again that a helper procedure is useful in doing the house cleaning that concludes the computer's turn (updating the computer's overall score, reset its turn score and reenabling the buttons).

The game should now be functional so try playing a few rounds against the computer. Remember to use the logging library that you read about in last unit's preparation to help diagnose what is happening when your program doesn't behave as expected.

Although the game (hopefully) works roughly as intended you may find the computer turn to be quite hard to follow as it happens so quickly that you can hardly see the die rolls and the label updates. Let's address that by refactoring the computer turn:

* Get rid of the while loop (but not its contents!) and make the computerTurn method handle a single roll of the computer's turn
* If the computer can and does decide to roll again create a timed event that will do so after an appropriate delay (say 500 ms). To accomplish this, you can use Handler.postDelayed, an example of which can be seen on StackOverflow.



---


<a name="A-3"></a>
### A-3-




---


<a name="A-4"></a>
### A-4-





---


<a name="A-5"></a>
### A-5-




---


