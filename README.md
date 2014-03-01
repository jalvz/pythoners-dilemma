pythoners-dilemma is a simple implementation of a well known [cooperative game] (http://en.wikipedia.org/wiki/Prisoner%27s_dilemma).


The purpose is to show how the game evolves with different strategies (This means that you 
can not expend hours playing to this. An interactive version wouldn't be too much work, thou.)


Lets kick some matches with a Prissoner's Dilemma matrix and 100 rounds per match...    
    
    
    
    play_with('Nice', 'TitForTat')
    # These 2 guys get along very well... 
    #
    # Alice scored 151 points with strategy Nice
    # Bob scored 151 points with strategy TitForTat


    play_with('Nice', 'Naive')
    # Naive tries to get advantage of what works...
    #
    # Alice scored 82 with strategy Nice
    # Bob scored 139 with strategy Naive
    
    
    play_with('Nice', 'NaiveProber')
    # And Naive Prober tries aggressively...
    #
    # Alice scored 41 with strategy Nice
    # Bob scored 167 with strategy NaiveProber


    play_with('NaiveProber', 'Majority')
    # But the NaiveProber can't compete against a Majority!
    #
    # Alice scored -102 with strategy NaiveProber
    # Bob scored 24 with strategy Majority
    
    
    play_with('SmoothTitForTat', 'TitForTwoTats')
    # On the other hand, Tit For Tat cousins destroy each other...
    #
    # Alice scored -182 points with strategy SmoothTitForTat
    # Bob scored -155 points with strategy TitForTwoTats
    
    
    play_with('Selfish', 'Crazy')
    # A Selfish guy gets a good chance against someone who doesn't know
    # too much about the game...
    #
    # Alice scored 66 points with strategy Selfish
    # Bob scored -12 points with strategy Crazy
    
 
    play_with('Alternate', 'Majority')
    # Some really simple strategies can get good results sometimes...
    #
    # Alice scored 250 with strategy Alternate
    # Bob scored 100 with strategy Majority
    
    
    play_with('SpitefulProbabilisticAdaptive', 'Alternate')
    # But there are smart strategies out there...
    #
    # Alice scored 53 with strategy SpitefulProbabilisticAdaptive
    # Bob scored -79 with strategy Alternate


    play_with('SpitefulProbabilisticAdaptive', 'Selfish')
    # ...who doesn't let the bad guys win...
    #
    # Alice scored -193 with strategy SpitefulProbabilisticAdaptive
    # Bob scored -193 with strategy Selfish


    play_with('SpitefulProbabilisticAdaptive', 'TitForTat')
    # ...look for a sustainable world...
    #
    # Alice scored 200 with strategy SpitefulProbabilisticAdaptive
    # Bob scored 200 with strategy TitForTat    
    
    
    play_with('SpitefulProbabilisticAdaptive', 'Nice')
    # ...and still can't avoid taking advantage of the weaker!
    #
    # Alice scored 161 with strategy SpitefulProbabilisticAdaptive
    # Bob scored -49 with strategy Nice



On top of this, we can simulate a population of strategies, put them to play, and see how the population evolve.
These are the rules:

1 - There is a initial queue filled randomly with strategies passed as input.

2 - It pulls the first 2 players of the queue and performs a game with 50 rounds per game.

3 - If one player reaches 70 points, 2 players with the same strategy will be added to the queue.
    At that time, if the other one has more than 50 points, 1 player with the same strategy will
    be added to the queue. Both scores reset.
    
4 - Repeat until queue is empty or number of generations limit has reached.
    


Lets do some tests with 200 generations each.



    # Mostly good guys...

    Generational('PrisonerMatrix', ['TitForTat', 'Alternate', 'Majority', 'Naive', 'Nice'], 200).start()

    # Population size after 200 generations is 329
    # 37.0% of the population are TitForTat
    # 34.0% of the population are Naive
    # 21.0% of the population are Majority
    # 3.0% of the population are Nice
    # 2.0% of the population are Alternate


    # Quite a lot genetic variability, eh? All the strategies managed to survive!

    
    # What about having mostly bad guys?

    Generational('PrisonerMatrix', ['TitForTwoTats', 'Alternate', 'Majority', 'Selfish', 'Spiteful'], 200).start()

    # Population size after 200 generations is 350
    # 54.0% of the population are Spiteful
    # 45.0% of the population are Majority
    
    
    # (Don't worry about 54 + 45 not beeing 100. The minorities are excluded from the output.)
    
    
    # And some good, some bad guys mixed togheter...    

    Generational('PrisonerMatrix', ['Nice', 'AlternateCCD', 'TitForTat', 'Selfish', 'Spiteful'], 200).start()

    # Population size after 200 generations is 324
    # 63.0% of the population are Spiteful
    # 34.0% of the population are TitForTat


    # Not a chance for Nice or AlternateCCD. Lets swap one of them with an adaptive strategy!
    # This DamainBasedAdaptive gathers information of the payout matrix to make his choices...

    Generational('PrisonerMatrix', ['Nice', 'DomainBasedAdaptive', 'TitForTat', 'Selfish', 'Spiteful'], 200).start()

    # Population size after 200 generations is 388
    # 35.0% of the population are TitForTat
    # 34.0% of the population are Spiteful
    # 27.0% of the population are DomainBasedAdaptive

        
    # DomainBasedAdaptive manages to survive, but SpitefulProbabilisticAdaptive does better...

    Generational('PrisonerMatrix', ['Nice', 'SpitefulProbabilisticAdaptive', 'TitForTat', 'Selfish', 'Spiteful'], 200).start()

    # Population size after 200 generations is 372
    # 45.0% of the population are SpitefulProbabilisticAdaptive
    # 27.0% of the population are Spiteful
    # 25.0% of the population are TitForTat
    

    # So far, so good. Now, lets move into a more challenging payout matrix...
    
    # The fearsome ChickenMatrix!

    Generational('ChickenMatrix', ['Nice', 'AlternateCCD', 'TitForTat', 'Selfish', 'Spiteful'], 200).start()

    # Population size after 25 generations is 0
    
    
    # Oops...


    # That one was too hard, no one survived! 
    # Lets try 100 rounds per match, to give the players more chances to replicate themselves...

    Generational('ChickenMatrix', ['Nice', 'AlternateCCD', 'TitForTat', 'Selfish', 'Spiteful'], 200, 100).start()

    # Population size after 200 generations is 360
    # 64.0% of the population are TitForTat
    # 35.0% of the population are Spiteful
    
    
    # Looks better, but not a huge difference comparing with the PrisonerMatrix.
    # Lets put into play our kick-ass adaptive strategy...

    Generational('ChickenMatrix', ['Nice', 'SpitefulProbabilisticAdaptive', 'TitForTat', 'Selfish', 'Spiteful'], 200, 100).start()

    # Population size after 200 generations is 384
    # 50.0% of the population are Spiteful
    # 25.0% of the population are SpitefulProbabilisticAdaptive
    # 25.0% of the population are TitForTat


    # Hummm... not too bad. The ChickenMatrix resolves the defeat-defeat situations with a high penalty.
    # The domain-based adaptive strategy should use this information to avoid mutual destruction and positionate itself something higher...

    Generational('ChickenMatrix', ['Nice', 'DomainBasedAdaptive', 'TitForTat', 'Selfish', 'Spiteful'], 200, 100).start()

    # And here we go!
    #     
    # Population size after 200 generations is 386
    # 41.0% of the population are DomainBasedAdaptive
    # 33.0% of the population are Spiteful
    # 24.0% of the population are TitForTat
    

    # That's cool, but really, nothing can beat a bunch of well-intentioned guys!

    Generational('ChickenMatrix', ['Naive', 'TitForTat', 'ProbabilisticAdaptive'], 200, 100).start()

    # Population size after 200 generations is 450
    # 34.0% of the population are ProbabilisticAdaptive
    # 33.0% of the population are TitForTat
    # 32.0% of the population are Naive
    
    # Did you see that? Birth rate is 20%-30% higher!! 
    



And that is everything so far. Thanks for coming!

