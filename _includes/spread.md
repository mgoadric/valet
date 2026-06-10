### Decision Spread

![Decision Spread Graph]({{ site.baseurl }}/assets/images/spread/Spread{{ page.title | replace:' ',''}}.png)

The figure above illustrates the normalized spread (y-axis) for each decision point (x-axis) of {{ page.title }}, summarized over 100 games with all MCTS players. At each decision point, a player evaluates the potential moves in terms of their potential final score. The spread is the difference between the best estimated move and the worst estimated move. This value is then divided by the highest spread found during the game to calculate the normalized spread.