Protocracy
=========

A prototype of democracy. For representative democracy just ain't good enough any more.

The basis of protocracy is formed in the idea of giving each individual equal influence in a shared government model. The system should be robust enough to withstand manipulation or corruption. At the same time it must not become a burden and a time sink for the individual.

At the heart of protocracy is the singular vote, placed by an individual. Intricately linked to this vote, is the motion the vote affects. The system must serve to distribute the motions to the voters and gather their votes.

Actions
----------
1. Place motion [ info, meta ]
2. Vote for motion [ decision, meta ]
3. Get motions for consideration [ individual ]
4. Verify motion [ motion ]
5. Verify vote [ key, motion ]
6. Get passed motions [ date ]

..............................................................................

1. Place motion
---------------

  Parse request () => (token, motion)
		Authorize request (token) => (citizenship)
      If failure (citizenship)
        Return Authorization Failed
      Throttle motions (citizenship) => (blocked)
        If (blocked)
          Return Motion Throttle block
        If Not (blocked)
          Store motion (citizenship, motion) => (motion)
            Return response (motion)
            Create motion vote context (motion) => (voters)
              Notify voters (voters, motion)

2. Vote for motion
------------------

  Parse request () => (token, motion, vote)
    Authorize request (token) => (citizenship)
      If failure (citizenship)
        Return Authorization Failed
      Store vote (citizenship, motion, vote)
        Return response (OK)
        Count votes (motion) => (state)
          If voting complete (state)
            Notify motion creator (motion, state)
            Lookup voters (motion) => (voters)
              Notify voters (voters, motion, state)
        
3. Get motions for consideration 
--------------------------------

Parse request () => (token)
  Authorize request (token) => (citizenship)
    If failure (citizenship)
      Return Authorization Failed
    Lookup open votes (citizenship) => (motions)
      Return response (motions)


4. Verify motion
----------------

Parse request () => (token, motion)
  Authorize request (token) => (citizenship)
    If failure (citizenship)
      Return Authorization Failed
    Lookup motion (citizenship) => (motion, state)
      Return response (motion, state)

5. Verify vote
--------------

Parse request () => (token, vote, motion)
  Authorize request (token) => (citizenship)
    If failure (citizenship)
      Return Authorization Failed
    Lookup motion (motion) => (motion)
      Verify vote (vote, motion) => (match)
        Return response (match)

6. Get passed motions
---------------------

Parse request () => (token, dateFrom, dateTo, continueFromMotion)
  Authorize request (token) => (citizenship)
    If failure (citizenship)
      Return Authorization Failed
    Query motions (dateFrom, dateTo, continueFromMotion) => (motions)
      Return response (motions)
