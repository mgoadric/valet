### Card Movement

![Card Movement Graph]({{ site.baseurl }}/assets/images/movement/{{ page.title }}.svg)

The diagram above shows a sample game inscription generated from a random rollout for {{ page.title }}. Visibility is applied to card locations rather than cards themselves. Public card locations are shown in green, hidden card locations are yellow, private card locations are blue, and memory locations are light gray. Source locations for cards with distinct backs are denoted with a bold border. Named rectangles denote the ownership of the card locations, by either players or the game table. 

Directional edges show how cards move between card locations, changing visibility as they move. Edges are colored green when public information remains public for all players. Red edges visualize when cards move from public to either private or hidden locations. Shared imperfect information is drawn with blue edges, where private cards are moved to other player's private or hidden locations. 

