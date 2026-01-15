---
title : "President"
image : "/assets/images/post/hearts.jpg"
date: 1880-08-08 11:12:58 +0600
description : "This is meta description"
tags : ["Climbing", "Highest Wins"]
num_players : 5
origin : China
deck : French
source : https://www.pagat.com/climbing/president.html
---

In Hearts, players play for themselves, with points assigned to the Heart-suited cards, along with the Queen of Spades. Hearts is a trick-avoidance game, such that the player with the lowest score wins, and players cannot lead Hearts until someone else has played one as a following card. 

### Rules of Play

A one-round game of Hearts for four players consists of thirteen tricks. First,
shuffle a standard deck of cards. Each player receives thirteen cards. For each
trick, players play one card to the trick. The first player will set the lead suit for
the trick, which subsequent players must follow suit if they can, otherwise they
may play any card from their hand. Also, the first player is restricted to not
play a card from the Hearts suit unless one has already been played. Once all
cards have been played, the player who played the highest card that matches
the suit of the led card will collect all the cards in the trick and become the
first player for the next trick. Once all tricks have been played, players earn
one point for each ♡ collected in tricks, plus 13 points if they collected the
Q♠. If a player happens to collect all ♡ and the Q♠, then they will Shoot the
Moon and instead subtract 26 points from their score. The player with the
lowest point value wins the game.

### RECYCLE Code

{% highlight racket %}
(game
 (setup  
  ;; Set up the players, 4 players each on their own team
  (create players 4)
  ;; Create the deck source
  (create deck (game iloc STOCK) 
   (deck (RANK (A, TWO, THREE, FOUR, FIVE, SIX, SEVEN, 
                EIGHT, NINE, TEN, J, Q, K))
         (COLOR (RED (SUIT (HEARTS, DIAMONDS)))
                (BLACK (SUIT (CLUBS, SPADES)))))))       
 
 ;; Stages of the game
 (do (
  (set (game points SCORE)
       (((SUIT : HEARTS) 1) 
        ((RANK : Q) (SUIT : SPADES) 13)))
  (shuffle (game iloc STOCK))
  (all player 'P
  (repeat 13
   (move (top (game iloc STOCK))
         (top ('P iloc HAND)))))
  (set (game str LEAD) NONE)
  (set (game sto BROKEN) 0)))

 ;; players play a round 13 times     
 (stage player
  (end (all player 'P (== (size ('P iloc HAND)) 0)))
               
  ;; players play a hand once
  (stage player
   (end (all player 'P (== (size ('P vloc TRICK)) 1)))
                      
   (choice (   

    ;; if first player and hearts not broken and have non-hearts cards
    ;;   play one of these, remember it in the lead spot, and end your turn
    ((and (== (game str LEAD) NONE)
          (== (game sto BROKEN) 0))
     (any (filter ((current player) iloc HAND) 'NH 
                  (!= (cardatt SUIT 'NH) HEARTS)) 'C     
      (do (
       (move 'C  (top ((current player) vloc TRICK)))               
       (set (game str LEAD) (cardatt SUIT (top ((current player) vloc TRICK))))))))
                        
    ;; if first player and hearts not broken and all cards hearts
    ;;   play any card, remember it in the lead spot, and end your turn
    ((and (== (game str LEAD) NONE)
          (== (game sto BROKEN) 0)
          (== (size (filter ((current player) iloc HAND) 'NH 
                            (!= (cardatt SUIT 'NH) HEARTS))) 0))
            
     (any ((current player) iloc HAND) 'C
      (do (
       (move 'C (top ((current player) vloc TRICK)))
       (set (game str LEAD) (cardatt SUIT (top ((current player) vloc TRICK))))))))
                        
    ;; if first player and hearts broken
    ;;   play any card, remember it in the lead spot, and end your turn
    ((and (== (game str LEAD) NONE)
          (== (game sto BROKEN) 1))
     (any ((current player) iloc HAND) 'C
      (do (
       (move 'C (top ((current player) vloc TRICK)))
       (set (game str LEAD) (cardatt SUIT (top ((current player) vloc TRICK))))))))
                        
    ;; if following player and cannot follow SUIT
    ;;   play any card, and end your turn
    ((and (!= (game str LEAD) NONE)
          (== (size (filter ((current player) iloc HAND) 'H 
                            (== (cardatt SUIT 'H) 
                                (game str LEAD)))) 0))
     (any ((current player) iloc HAND) 'C
      (move 'C (top ((current player) vloc TRICK)))))
                        
    ;; if following player and can follow SUIT
    ;;   play any card that follows SUIT, and end your turn
    (any (filter ((current player) iloc HAND) 'H 
                  (== (cardatt SUIT 'H)
                      (game str LEAD))) 'C
     ((!= (game str LEAD) NONE)
      (move 'C (top ((current player) vloc TRICK))))))))
               
   ;; after players play hand, computer wraps up trick
   (do ( 
    ;; solidfy card recedence
    (set (game points PRECEDENCE)
           (((SUIT : (game str LEAD)) 100)
            ((RANK : A) 14)
            ((RANK : K) 13) 
            ((RANK : Q) 12)
            ((RANK : J) 11)
            ((RANK : TEN) 10)
            ((RANK : NINE) 9)
            ((RANK : EIGHT) 8)
            ((RANK : SEVEN) 7)
            ((RANK : SIX) 6)
            ((RANK : FIVE) 5)
            ((RANK : FOUR) 4)
            ((RANK : THREE) 3)
            ((RANK : TWO) 2)))
      
    ;; determine who won the hand, set them first next time, and give them a point
    (set (game str LEAD) NONE)
    (cycle next (owner (max (union (all player 'P ('P vloc TRICK))) 
                            using (game points PRECEDENCE))))
      
    ;; if winner played trump and trump not broken, trump is now broken
    ((and (!= (size (filter (union (all player 'P ('P vloc TRICK))) 'PH  
                            (== (cardatt SUIT 'PH) HEARTS))) 0)
          (== (game sto BROKEN) 0))
     (set (game sto BROKEN) 1))
      
    ;; discard all the played cards
    (all player 'P
     (move (top ('P vloc TRICK)) 
           (top ((next player) vloc TRICKSWON)))))))
        
 ;; determine score
 (stage player
  (end (all player 'P (== (size ('P vloc TRICKSWON)) 0)))
  (do (
   ((== (sum ((current player) vloc TRICKSWON) using (game points SCORE)) 26)
    (dec ((current player) sto SCORE) 26))
                    
   ((!= (sum ((current player) vloc TRICKSWON) using (game points SCORE)) 26)
    (inc ((current player) sto SCORE) 
         (sum ((current player) vloc TRICKSWON) using (game points SCORE))))
                    
   (repeat all
    (move (top ((current player) vloc TRICKSWON))
          (top (game vloc DISCARD)))))))
 
 (scoring min ((current player) sto SCORE)))
{% endhighlight %}

### Branching Factor


