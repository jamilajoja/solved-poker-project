Download Link: https://assignmentchef.com/product/solved-poker-project
<br>
There are 3 substantial changes to make to the project to finish it.

[A] Prevent duplicate cards from being drawn from the deck.

[B] Let the user replace one of their cards in their hand, and auto replace a low card in the computer’s hand.

[C] Recognize if entire hand is all the same suit (a “flush”), and treat that as a winning hand.

A few definitions of my wording.




<ul>

 <li>The complete set of 52 cards, including 13 Hearts, 13 Clubs, 13 Spades, and 13 Diamond is called a “<strong>deck</strong>”. We use the cardArray to simulate the deck.</li>

 <li>There are 2 “<strong>players</strong>” the <strong>computer</strong>, and the <strong>player</strong>, also called the <strong>user</strong>.</li>

 <li>You have a while <strong>loop</strong>, and each time you go thru the <strong>loop</strong>, new cards are gotten and displayed, and one dollar is won or lost. Sometimes I call that a <strong>round</strong>. In a card game, that cycle would be called “<strong>a hand</strong>”.  However, the 5 cards the player gets is also called a <strong>hand</strong>, the player’s <strong>hand</strong>.  And the 5 cards the computer gets is the computer’s <strong>hand</strong>.  So in card games, a hand has 2 meanings. Our path thru the getting new cards, betting, and deciding a win or a loss is called “a <strong>hand</strong>”, as a short way of saying, “do you want to be dealt another <strong>hand</strong>”.  So both one cycle of the game as well as the smaller arrays of 5 cards are both called “a <strong>hand</strong>”.</li>

 <li>In real Poker, various special combinations of cards are given extra value. We are going to simulate just <strong>one</strong> of those.  It is called a <strong>flush</strong>.  A <strong>flush</strong> means all cards in a hand (the small array) are of the same suit, such as 5 Hearts.</li>

</ul>




If you have not done this already, it’s a good idea to add this line

Console.Clear();

at the very beginning of your main loop. It makes the game nicer to play, and avoids the problem in the Windows Console code with the colors getting broken after playing a few hands.

Now on to the 3 improvements:

—————————————————————————————————————————-




[A] The idea — Our Deck is made up of 52 unique card. During one cycle of the game, we deal out two “hands”, usually 5 cards, and as you will see later, sometimes one or two more cards.  That means for each cycle of the game, each pass through the main loop, we use between 10 and 12 cards.  They should never be duplicates. In a real deck of cards, if you take a card out and give it to a player, obviously it would be impossible to give out that same card again. So we will make our game comply. Note carefully that at the end of each “hand”, loop, we put all the cards back in the deck, so the same cards can be seen over and over again in subsequent loops, you just should never see the same card repeated in one turn.




[A] How to implement —

To do this, we will create a <strong>new property</strong> in our <strong>SuperCard Class</strong>. It will be a bool property, called  <strong>inplay</strong>  Every time <em>any</em> method in our CardSet Class  “gives out a card”, it will mark that card as <em>used</em> by setting that card’s inplay property to true.  But also, this means every time it selects a card to give out, it must first check if that card is in play already (is the inplay property already true?).  If it is, it must find another card that is not inplay.




At the beginning of each game round (pass through the while loop), we will “reshuffle” the deck.  For our game, all that means is we will set all the card used values (<strong>inplay</strong>) back to false, so all cards are again available for the next hand.




<strong>Steps:</strong>

1) In the SuperCard Class, add a new Get Set property of type bool, called <strong>inplay</strong>.  (When we give out a card, we will set it to true, when we “shuffle” the deck, we will set it to false.)

2) In the CardSet Class, create a new public method called <strong>ResetUsage</strong>. This method needs to loop through all 52 cars in the array and for each one, set their <strong>inplay</strong> property to false.

3) In the CardSet Class, modify your <strong><em>existing</em></strong> GetCards method, to

* verify that each card it gives out is not already in use.  (In your loop that gives out 5 (or whatever number was passed in) cards, use brute force.  You do not need a computer efficient way of doing this. Just keep looking for the next card using the Random from 0-52, but if it picks a number of a card that is in use, do nothing, just keep your loop going until you do get 5 unused ones.  So instead of your loop going thru 5 times, sometimes it might take 10, 15, even 25 times until it finds 5 unused cards.  That’s ok.

* when it does find a card that is not in use, it should copy it in the return array, but also, your code needs to mark that card’s inplay property to true so that it won’t be used again.

* This loop will require some thinking, but my version is only 20 lines, of which 7 are just {‘s  or }’s  so that’s 13 lines of real code.  You can

<ul>

 <li>use a while loop and keep track until you have succeeded in getting 5 cards,</li>

 <li>or you can use a for loop that goes from 0 to 4, and then set the counter back -1 when you pick a card that is in use,</li>

 <li>or in the loop, you can have a 2<sup>nd</sup> loop that loops until it finds a card.</li>

 <li>I won’t be giving you help on this aspect, you need to make this work.</li>

</ul>




4) In program.cs, at the very beginning of each round ( hand,  the while loop), call your CardSet ResetUsage method so that all cards are again available.




At this point, check your program carefully.  Look at each round, and verify that none of the 10 cards showing across the 2 hands are the same.  It’s easy to check visually, check by just looking at the red cards, then the blue, then black, and then dark red.  Set your hand size to 10 and then it will be very easy to spot if you are giving duplicates, as there will be 20 cards in play. Also, play at least 4 turns in a game to make sure you are putting the cards back in play. If you do not, your program will probably get stuck in an infinite loop.




<strong>That is the end of [A]</strong>







[B] The idea — Now we want to allow the user and the computer to replace one card to improve their hand before deciding who won.




[B] How to implement —

First add a new method in CardSet class.

Using very similar code to your GetCards method, add a new method

public SuperCard GetOneCard()  // gets one card from the deck of 52.

It <strong>must</strong> have similar logic to make sure the card it returns is not already in use, and it must set the property of the card it does return to be true.  Note also that this method returns a <em>single</em> SuperCard, and not an <em>array</em> of cards.




Now we are going to let the player replace any <strong><em>one</em></strong> card, or <strong>none</strong>.  In your main game loop, right after you display the cards for the player (write them to the console), add a new method

PlayerDrawsOne(myHand, myDeck);

// your name for these 2 parameters, the players small array, and the object you created for the CardSet class might be different







Now, in Program.cs, implement this PlayerDrawsOne  method.  Start by asking the user which card they want to replace, and allow them to type 1,2,3,4 or 5, etc, which is to indicate which card they want to replace, or they can enter a 0 if they don’t want to change any card.  If they <strong><em>do</em></strong> want to change a card, call your new GetOneCard method to get a new card, and <strong>replace</strong> that one card object in their hand array corresponding to the number they selected.  Remember, arrays are “by reference” types, so when you pass in the array  (PlayerDrawsOne(<strong>myHand</strong>, myDeck);

and modify the myHand array there, you don’t have to return anything, as the original hand array is now modified too.  <strong>Note</strong>, if they say, for example, they want to replace their 3<sup>rd</sup> card, in the array that is the [2] card, as humans count from 1, computers count from 0.




Get this working before going forward. You should be able to play the game, and pick any of your cards to be replaced, or select a 0 and replace none.  You will now have an advantage over the computer, as you can swap your lowest card, and the computer is stuck with the cards it got in the deal. But that’s not fair, so we need to let the computer pick a new card too.




So, right after your call to the method that lets the user pick a new card, and another new method call to <strong><em>let the computer</em></strong> also update its hand:




ComputerDrawsOne(computerHand, myDeck);

// your name for these 2 parameters, the computers small array, and the object you created for the CardSet class might be different




Then implement that method in program.cs. This new method should calculate the <strong><em>lowest card Rank</em></strong> value in the computer’s hand. Note it doesn’t matter if there are 2 of the same lowest rank, just get an index pointing to <em>one</em> of the cards for which no other card in the hand has a lower rank. Now that you have identified a lowest card:

* If that card is of Rank less than 8, then call your GetOneCard method and replace <em>that</em> card.

* If the lowest card’s Rank is equal to, or greater than 8, <strong>do not</strong> change any card for the computer’s hand.

That is our “artificial intelligence”! The computer now makes a decision as to if, and which one card to replace.




We want the user to see the cards again after we may have changed one card in both hands. So right after calling those 2 methods to update the user’s and the computer’s hand, again make calls to <strong>sort</strong> the hands, and a call to <strong>re-display</strong> hands. The player should be able to see both the original hands, and the 2 updated hands.  Your code then proceeds as before, and decides if it was a win or a loss, there is no change to that logic or flow.




So at this point, your code flow inside your loop should look something like:




Console.Clear();

myDeck.ResetUsage();

Console.BackgroundColor = ConsoleColor.Blue;

Console.ForegroundColor = ConsoleColor.White;




SuperCard[] computerHand = myDeck.GetCards(howManyCards);

SuperCard[] playersHand = myDeck.GetCards(howManyCards);




//sort players and computers hand

Array.Sort(computerHand);

Array.Sort(playersHand);




//display card hands

Console.WriteLine();

DisplayHands(computerHand, playersHand);




//ask user to change one card

PlayersDrawsOne(playersHand, myDeck);

//computer changes one card

ComputerDrawsOne(computerHand, myDeck);




//sort players and computers new hands

Array.Sort(computerHand);

Array.Sort(playersHand);




Console.WriteLine();

//display new card hands

DisplayHands(computerHand, playersHand);




bool won = CompareHands(computerHand, playersHand); //determines who wins the hand







<strong>Test program well</strong>, looking carefully at the cards to make sure you are not getting the same card twice in a round, and that the computer is replacing the lowest card, or not replacing a card if it is greater or equal in Rank to a 7.  Make sure the player’s one card replacement replaces the correct card, or no card if you enter a 0.




<strong>That is the end of [B]</strong>







[C] The idea — Recognize if a hand is all of the same suit (a “flush”), and treat that as a winning hand.

[C] How to implement —

You must use a new <strong>Interface</strong> in your project, a system supplied one, IEquatable&lt;T&gt;, and then using that, decide if either the player or computer’s hand is a flush, and factor this into deciding who wins the hand (round).  A flush is when all cards in the hand are the same suit.

In general, this is what needs to be accounted for in the code.

<ul>

 <li>The computer “Artificial Intelligence” does not need to be clever enough to try for a flush, so the method ComputerDrawsOne method does <strong>not</strong> have any logic to try and pick one card to discard and attempt to <em>get a</em></li>

 <li><strong>BUT</strong>, the computer code <em>should be</em> smart enough to know <strong>if it received a flush</strong> on the first deal, and if it did, <em>then it should not draw a new card</em>, since a flush is always a winning hand (in our game).</li>

 <li>The player must use their eyes and brain, and decide if they get dealt, say for example, 4 of 5 cards all of a matching suit, maybe they want to try for a flush. So it’s up to the <em>player</em> (the human, not the code) to decide if they want to replace the one card that does not match the suit of the other 4. Also, if they have a flush, of course it’s up to the player to be clever enough not to draw any new card. In any case, <strong>no code changes</strong> are required for the player’s code logic, it’s up to the brain of the player.</li>

 <li>Changes you will have to do include: (Don’t make this hard!)</li>

 <li>Modify your method <strong>CompareHands</strong> to <strong><em>first</em></strong>, check for flushes.

  <ul>

   <li>If the computer has a flush, it wins, regardless. (Even if the user draws a flush, Computer wins all ties). Return from the method with a false.</li>

   <li>Assuming the computer did not have a flush, if the player has a flush, they win. Return from the method with a true.</li>

   <li>If neither have a flush, then just fall into your existing code that counts up the value of the cards is used to decide who wins.</li>

   <li>In theory, if the computer has a flush on the initial deal, you could stop that hand right then. But (a) that makes the game more complicated to code, and (b) in real life, you might not be able to see the computer’s (dealer’s) hand, so just let the game cycle through where the player gets to replace one card, even if they are wasting their time.</li>

  </ul></li>

</ul>

<strong>My suggested steps:</strong>

First, add the IEquatable interface to the SuperCard Class (<strong>it is a requirement</strong> that you do this!)

That requires you to then support that interface with a new method in the SuperCard Class definition.  We will design ours to tell us if the card object passed in to this card object, is the <strong>same suit</strong> as the card object executing the method. Return <strong>true</strong> or <strong>false</strong> based on the suit value of each of the cards, true means they are both of the same suit.

public bool Equals(SuperCard otherCard)

{

Your code here!  The IEquatable interface required “Equals” method returns a true or false.

}




Next, please create a method with this signature in Program.cs:

private static bool Flush(SuperCard[] hand) // can be called passing in either

//the players hand or the computers hand

{

// your code here

}




It should return a false if all the cards in a hand are not the same suit, and a true if all the cards are the same suit.  That method <strong>mus</strong>t make use of the new IEquatable capability that SuperCards now has.

(Assuming we are using 5 cards, you only need to make 4 comparisons, so I think a for loop will be better than a foreach loop.)

Using the comparison method of our new IEquatable  in SuperCard will look something like (your variable names may be different):

oneOfTheCardsInTheHand.Equals(AnotherCardInTheHand)

You access this new compare method by calling the .Equals method of one card and passing in the other card you want to compare. So build a loop, and compare each card in the passed in hand (array) to each other and return false if any of them don’t match any of them.

10) Now use that new Flush method in your CompareHands method to factor in the decisions about who won.  I put code in my CompareHands method that does a  Console.WriteLine(“It’s a flush!”); whenever the Flush method does find a flush, just to make it easier to debug. Please do the same. CompareHands should return false (meaning computer won) if the computer has a flush. If it does not, and the player has a flush, then the player wins.  If neither do, then, as before, the sum of the hands Rank values decides the winner.

(11) Now modify the logic in the ComputerDrawsOne method to also use the new Flush method to first check if the computer already has a flush, in which case if it does, <em>it should not draw</em> a new card.

<strong>TEST PROGRAM!!</strong>

This takes a while to test, I suggest you change your number of cards variable to a 3, as this will make the chances of getting flushes much higher.  You need to test these conditions

<ol>

 <li>No flushes, user’s hand has higher point value than computer</li>

 <li>No flushes, user’s hand has lower point value than computer</li>

 <li>Computer has flush, user does not</li>

 <li>User has flush, computer does not</li>

 <li>Both have a flush</li>

</ol>




<strong>At the end: </strong>

<ul>

 <li><strong>[A] No duplicate cards used </strong></li>

 <li><strong>[B] User and computer draw one card functionality all correct </strong></li>

 <li><strong>[C] Implements the </strong><strong>IEquatable functionality correctly in SuperCard, uses the resulting .Equals method and correctly deals with flushes </strong></li>

</ul>








