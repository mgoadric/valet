### Information Flow

![Card Movement Graph]({{ site.baseurl }}/assets/images/movement/{{ page.title | replace:' ',''}}.svg)

The diagram above visualizes information flow in {{ page.title }} using a single random rollout in [CardStock](https://github.com/mgoadric/cardstock). Visibility
is attached to card locations, so all cards in a 
location share the same visibility;
information is revealed or concealed as cards move between locations and ownership changes.

**Public** locations are shown in green, **hidden** locations in yellow, **private** locations in blue, and memory locations in light gray. Locations containing cards with distinct **backs** are indicated with bold borders. Rectangles denote ownership (players or table), and directed edges show card movement between locations and resulting visibility changes. **Taken** information is shown with red dotted edges (public to private/hidden), and **shared** information with blue dashed edges (private cards transferred to another player).

