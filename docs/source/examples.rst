Examples
========

iterall
--------

.. code-block:: python

  import fuzzquery as fq
  
  data = """ 
  I headed homeward to meet with the Wardens. 
  When I arrived, I was greeted by a homely man that told me the homestead was awarded 5 million dollars.
  We intend to use some of the homage to create a homeless ward. 
  The first piece of furniture will be my late-friend Homer's wardrobe.
  """
  queries = ('home{5}', 
             'home{4} ward{4}', 
             '{1}ward{!2}{2}', 
             'hom{5} {?} wa{!1}{5}')
  
  for query, span, match in fq.iterall(data, queries, ci=True):
      if query: print(f'\n{query.upper()}')
      print(f'  {match}')

.. code-block:: console

  HOME{5}
    homeward
    homely
    homestead
    homeless
    Homer's

  HOME{4} WARD{4}
    homeless ward
    Homer's wardrobe

  {1}WARD{!2}{2}
    Wardens
    awarded
    wardrobe

  HOM{5} {?} WA{!1}{5}
    homeward to meet with the Wardens
    homely man that told me the homestead was
    homage to create a homeless ward
    Homer's wardrobe
