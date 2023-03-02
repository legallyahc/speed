Figuring out metro speedrun stuff and expanding to multiple groups of
people
================
AHC
2023-03-02

## Initial thoughts: focus on one group

It is best to start at the longest extremity to minimize the amount of
distance that is doubled over.

![STM Metro Map](stm_map.png)

For 2022, this meant beginning at **Angrinon**, transfer at
**Berri-UQAM**, up to **Cote-Vertu**, the entire Blue line, up to
**Montmorency**, down to the **Berri-UQAM** once more, a lil hop on the
yellow, and then finally to **Honore-Beaugrand**. See the time
predictions below.

    ## # A tibble: 20 × 3
    ##    `TIME TRACKING`                                   `Estimated Time` Actual T…¹
    ##    <chr>                                             <time>           <time>    
    ##  1 START @ Angrignon                                 12:30            12:30     
    ##  2 OFF TRANSFER @ Berri-UQAM                         12:49            12:49     
    ##  3 ON TRANSFER @ Berri-UQAM                          12:55            12:55     
    ##  4 OFF TRANSFER @ Côte-Vertu                         13:17            13:20     
    ##  5 ON TRANSFER @ Côte-Vertu                          13:23            13:22     
    ##  6 OFF TRANSFER @ Snowdon                            13:32            13:31     
    ##  7 ON TRANSFER @ Snowdon                             13:41            13:36     
    ##  8 OFF TRANSFER @ Saint-Michel                       13:57            13:52     
    ##  9 ON TRANSFER @ Saint-Michel                        14:06            13:57     
    ## 10 OFF TRANSFER @ Jean-Talon                         14:10            14:01     
    ## 11 ON TRANSFER @ Jean-Talon                          14:16            14:03     
    ## 12 OFF TRANSFER @ Montmorency                        14:30            14:17     
    ## 13 ON TRANSFER @ Montmorency                         14:36            14:22     
    ## 14 OFF TRANSFER @ Berri-UQAM                         15:01            14:43     
    ## 15 ON TRANSFER @ Berri-UQAM                          15:09            14:50     
    ## 16 OFF TRANSFER @ Longueuil-Université-de-Sherbrooke 15:15            14:56     
    ## 17 ON TRANSFER @ Longueuil-Université-de-Sherbrooke  15:23            15:00     
    ## 18 OFF TRANSFER @ Berri-UQAM                         15:29            15:06     
    ## 19 ON TRANSFER @ Berri-UQAM                          15:35            15:07     
    ## 20 END @ Honoré-Beaugrand                            15:52            15:25     
    ## # … with abbreviated variable name ¹​`Actual Time`

## Co-op Rules

1.  A team must all begin in the same place.
2.  A team must all end in the same place.

-   People are allowed to leave the speedrun part way through, if they
    desire

3.  A team cannot divide into more groups than allowed.
4.  Each station a group visits contributes to their group total.
5.  Time begins when the first member of a team boards a train.
6.  Time ends when the group is all together (and all stations have been
    visted).
7.  For our purposes, we will begin at Berri-UQAM

-   Aside: I believe that the more ideal starting place would have been
    an extremity like **Honore-Beaugrand**, particularly in a two group
    run, but may make less snese in a three group or larger run
    (effectively more group time is used on this leg).

## Two-group team approach

For our situation, this would have two competing teams. Find below
Laura’s table of times for each segment:

![Laura’s Map](laurasmap.jpg)

    ## # A tibble: 11 × 2
    ##    segments times
    ##    <chr>    <dbl>
    ##  1 JT-O1       14
    ##  2 JT-B1        5
    ##  3 JT-B2       12
    ##  4 JT-BQ        9
    ##  5 B2-O2       11
    ##  6 B2-LG        7
    ##  7 LG-BQ_gr    10
    ##  8 LG-BQ_or    10
    ##  9 BQ-Y1        6
    ## 10 BQ-G1       17
    ## 11 LG-G2       11

The main time sink is and will be the **Honore-Beaugrand** leg, taking
17 minutes. Following from prior years, it would be ideal to minimize
transfers as they are inherently time losses. There is also the matter
ending in the same place! Arg.

Assuming no transfer time (teleporting between each and every train) and
no double back, we could make it in 56 minutes.

Let us eliminate the isolated branches extending from Berri-UQAM:

``` r
group1 <- times[10]*2 + 3
  # To Honore-Beaugrand and back, plus the typical 3 minute reversal time
group2 <- times[9]*2 + 3
  # To Longeuil and back, plus reversal
```

If the team already on the green line continues, we can avoid another
transfer. Let us have the other group continue to **Montmorency**.

``` r
group1 <- group1 + times[7] + times[11]*2 + 3
  # Intial time, BQ-LG, LG to Angrinon and back, and a 3 minute reversal time
group2 <- group2 + 9 + times[4] + times[1]*2 + 3
  # This takes the initial time, a maximum 9 minute transfer, JT-BQ, JT-O1*2 to double back to Jean-Talon, and a 3 minute reversal time
```

Our groups are now at **Lionel-Groulx** in 72 minutes and **Jean-Talon**
64 minutes, respectively. For ease of meeting, I think it could be ideal
to have *Group 2* take the Orange line segment between **Lionel-Groulx**
and **Berri-UQAM**, which would add 29 minutes with the least opportune
transfer, putting *Group 2* at 93 minutes at **Jean-Talon**. I think
this is the best solution, let’s run with it further.

With this now great disparity in time, *Group 1* has plenty of time to
take the western side to **Cote-Vertu** and down to Snowdon.

``` r
group1 <- group1 + 9 + times[6] + times[5]*2 + 3
  # Intial time plus max transfer plus LG->Snowdon plus Snowdon->Cote-Vertu*2 plus train turnaround time
group2 <- group2 + times[8]*2 + 9
  # Catching up to the calculations just made above
```

Now our groups are **Snowdon** and **Jean Talon**, at 113 and 93 minutes
respectively, a 20 minute difference. Providing the trip from **Jean
Talon** to **Saint-Michel** will eliminate this difference:

``` r
group2 <- group2 + 9 + times[2]*2 + 3
  # Initial time plus worst transfer plus time to Saint-Michel and back, plus turn around time.
```

Now *Group 2* is at **Jean-Talon** going Westbound at 115 minutes, while
*Group 1* is at **Snowdon** at 113 minutes. Even with the worst transfer
time of 9 minutes (total 122mins), the group will be able to meet
halfway through the Blue line. Good communication will be necessary, but
assuming half time of the leg between **Snowdon** and **Jean-Talon**,
that would give a time of 121-ish minutes.

``` r
group1 <- group1 + 9 + times[3]/2 + 2
  # Worst transfer and halfway split plus two mins to meet other group
group2 <- group2 + times[3]/2 + 2
  # Just halfway split plus two mins to meet other group
```

This provides a difference of times of 7 minutes, with *Group 1*
arriving at this hypothetical midpoint at 130 minutes and *Group 2* at
123 minutes. *Group 1* could likely travel an extra station or two, but
this will rely heavily on good communication between the groups.

I feel that this is the ideal routing, maintaining an equivalence of
times between the groups.

\<\<\<\<\<\<\< HEAD ### Ideal two-group routing *Group 1*:
**Berri-UQAM** -> **Honore-Beaugrand** -> **Angrinon** ->
**Lionel-Groulx** -> **Cote-Vertu** -> **Snowdon** -> Western Blue Line
station

*Group 2*: **Berri-UQAM** -> **Longeuil-Universite-Sherbrooke** ->
**Berri-UQAM** -> **Montmorency** -> **Jean-Talon** -> **Saint-Michel**
-> Western Blue line station

With a maximum time of 130 minutes, from *Group 1*.

## Three-group team approach

I feel that the ideal approach will be to minimize initial transfers and
then bringing the three groups back together will result in all parts of
the metro being covered.

So, let us have three groups: One that shall cover all of the Green line
(BQ -> HB -> AG -> LG), one that will go straight to *Cote-Vertu*, and
another that will cover the Yellow line and then to *Montmorency*.

``` r
# Green line
group1 <- times[10]*2 + 3 + times[7] + times[11]*2

# Cote-Vertu
group2 <- times[8] + times[6] + times[5]*2 + 3
  
# Yellow and Montmorency
group3 <- times[9]*2 + 3 + 9 + times[4] + times[1]*2 + 3
```

This now puts *Group 1* at **Lionel-Groulx** at 69 minutes, *Group 2* at
**Snowdon** at 42 minutes, and *Group 3* at **Jean-Talon** at 64
minutes. As *Group 2* is ahead of the next fastest group (*3*) by 22
minutes, it should do the run down the Blue line.

``` r
# Blue line
group2 <- group2 + 9 + times[3] + times[2]*2 + 3
```

Now, in this hypothetical, *Group 2* and *Group 3* are reunited at
**Snowdon**, though *Group 3* is already on the train (like,
technically, I guess). Let us investigate how long it takes for them to
get to **Berri-UQAM**.

``` r
# Lionel-Groulx to Berri-UQAM (already on the green line)
group1 + times[7]
```

    ## [1] 79

``` r
# Snowdon to Berri-UQAM (need to transfer)
  # At this point, Group 2 is equivalent to Group 3.
group2 <- group2 + times[4] + 9
group2
```

    ## [1] 94

Hm, that’s a bit funky. This would lead us to be limited by the group
coming from **Snowdon**. How does it look if we put *Group 1* on the
orange line?

``` r
# Orange line Lionel-Groulx to Berri-UQAM
group1 <- group1 + times[7] + 9
group1
```

    ## [1] 88

Now that’s much closer. Our ideal meeting station may be **Sherbrooke**,
with the 6 minute difference becoming more like 2-3 minutes, enough time
for whichever team arrives first to go to the other platform.

### Ideal route for three groups:

*Group 1*: **Berri-UQAM** -> **Honore-Beaugrand** -> **Angrinon** ->
**Lionel-Groulx** -> **Sherbrooke**

*Group 2*: **Berri-UQAM** -> **Cote-Vertu** -> **Snowdon** ->
**St-Michel** -> **Jean-Talon** -> **Sherbrooke**

*Group 3*: **Berri-UQAM** -> **Longeuil-Universite-Sherbrooke** ->
**Berri-UQAM** -> **Montmorency** -> **Sherbrooke**

# With a maximal time of 94 minutes (from *Group 2*).

## Three-group team approach

I feel that the ideal approach will be to minimize initial transfers and
then bringing the three groups back together will result in all parts of
the metro being covered.
