---
title : "President"
image : "/assets/images/post/president.jpg"
attribution : "By Lưu Ly - Own work, CC BY 3.0, https://commons.wikimedia.org/w/index.php?curid=5578630"
date: 1880-08-08 11:12:58 +0600
description : "This is meta description"
tags : ["Climbing", "Highest Wins"]
num_players : 5
origin : China
deck : French
source : https://www.pagat.com/climbing/president.html
---

In President, players play sets of cards from their hand that are higher in precedence than what 
was played previously.
Players can pass and then rejoin the game at any time, however if all players pass in sequence, then the last
player to play cards starts with a new set.
The goal is to eliminate all cards from your hand before other players.

### Rules of Play

> Rules go here

{% include movement.md %}

### RECYCLE Code

{% highlight racket %}
(game
 (declare 5 'NUMP)
 (setup 
  (create players 'NUMP)
  (create deck (game iloc STOCK) (deck 
   (RANK (ACE, TWO, THREE, FOUR, FIVE, SIX, SEVEN, 
          EIGHT, NINE, TEN, JACK, QUEEN, KING))
   (COLOR (RED (SUIT (HEARTS, DIAMONDS)))
          (BLACK (SUIT (CLUBS, SPADES)))))))         
 (do (
  (shuffle (game iloc STOCK))
  (set (game sto POINTS) 2)
  (set (game points PRECEDENCE) 
   (((RANK : TWO) 15) ((RANK : ACE) 14) ((RANK : KING) 13) ((RANK : QUEEN) 12)
    ((RANK : JACK) 11) ((RANK : TEN) 10) ((RANK : NINE) 9) ((RANK : EIGHT) 8)
    ((RANK : SEVEN) 7) ((RANK : SIX) 6) ((RANK : FIVE) 5) ((RANK : FOUR) 4)
    ((RANK : THREE) 3)))))
 (stage player 
  (end (== (size (game iloc STOCK)) 0)) 
  (do (
   (move (top (game iloc STOCK))
         (top ((current player) iloc HAND))))))  
 (stage player
  (end (all player 'P (== (size ('P iloc HAND)) 0)))
  (choice (
   ((== (size (game vloc PLAY)) 0)
    (any (partition RANK ((current player) iloc HAND)) 'PAR
     (any (subsets 'PAR) 'S 
      (do (
       (set (game sto SIZE) (size 'S))
       (repeat all
        (move (top 'S) 
              (top (game vloc PLAY)))))))))
   ((> (size (game vloc PLAY)) 0)
    (any (partition RANK ((current player) iloc HAND)) 'PAR
     (any (subsets 'PAR) 'S 
      ((and (== (size 'S) (game sto SIZE))
            (> (score (top 'S) using (game points PRECEDENCE))
               (score (top (game vloc PLAY)) using (game points PRECEDENCE))))
       (repeat all 
        (move (top 'S) 
              (top (game vloc PLAY))))))))
   ((or (> (size (game vloc PLAY)) 0)
        (== (size ((current player) iloc HAND)) 0))
    (inc (game sto PASSING)))))
  (do (
   ((== (game sto PASSING) (- 'NUMP 1))
    (do (
     (set (game sto PASSING) 0)
     (repeat all 
      (move (top (game vloc PLAY))
            (top (game vloc DISCARD)))))))
   ((and (== (size ((current player) iloc HAND)) 0)
         (== ((current player) sto SCORE) 0))
    (do (
     (set ((current player) sto SCORE) (game sto POINTS))
     ((> (game sto POINTS) 0)
      (dec (game sto POINTS)))))))))
 (scoring max ((current player) sto SCORE)))
{% endhighlight %}

### Branching Factor


