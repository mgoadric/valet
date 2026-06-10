### Lead History

![Lead History Graph]({{ site.baseurl }}/assets/images/lead/Leads{{ page.title | replace:' ',''}}.png)

The figure above illustrates the normalized average lead history (y-axis) for each decision point (x-axis) of {{ page.title }}, summarized over 100 games with all MCTS players. At each decision point, a player evaluates the potential moves in terms of their potential final score. For the best estimated move, they record the estimated score for all players, assigning them normalized rank estimates, where 1 is first place, and 0 is last place. Across 100 games, these histories are grouped according to final game rank, and then averaged.