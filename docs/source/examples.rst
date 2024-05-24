Examples
--------

.. code-block:: python

  import fuzzquery as fq
  
  data = """ 
  I headed homeward to meet with the Wardens. 
  When I arrived, I was greeted by a homely man that told me the homestead was awarded 5 million dollars.
  We intend to use some of the homage to create a homeless ward. 
  The first piece of furniture will be my late-friend Homer's wardrobe.
  """
  queries = ('hom{5} {?} wa{!1}{5}', 
             'home{5}', 
             '{1}ward{!2}{2}', 
             'home{4} ward{4}')
  
  for query, span, match in fq.iterall(data, queries, ci=True):
      if query: print(f'\n{query.upper()}')
      print(f'  {match}')
