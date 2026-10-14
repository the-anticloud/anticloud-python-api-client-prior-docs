# PYTHON_API_CLIENT_PRIOR_DOCS

![licence](https://img.shields.io/badge/licence-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![checks](https://img.shields.io/badge/checks-unknown_PASS-brightgreen)

> Governed Anticloud packaging of the upstream project `PYTHON_API_CLIENT_PRIOR_DOCS` in category **ONLINE_RETAIL**. check results: see ISOLATED_LAB_RESULTS. Every number below traces to a named file + run stamp; nothing is borrowed from other projects.

**Upstream:** PYTHON_API_CLIENT_PRIOR_DOCS · **Upstream pin:** `70a068d52cd329bdbb478ffa5020bdfd2f0a3a83` · **Category:** ONLINE_RETAIL · **Vendor:** Anticloud FZ LLE · **Licence:** MIT

---

## What This Project Does

*****************
RecombeeApiClient
*****************

A Python 3 client for easy use of the `Recombee <https://www.recombee.com/>`_  recommendation API.

If you don't have an account at Recombee yet, you can create a free account `here <https://www.recombee.com/>`_.

Documentation of the API can be found at `docs.recombee.com <https://docs.recombee.com/>`_.

=============
Installation
=============

Install the client with pip:

.. code-block:: bash

    $ pip install recombee-api-client

========
Examples
========

-------------
Basic example
-------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.exceptions import APIException
    from recombee_api_client.api_requests import *
    import random

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.US_WEST)

    #Generate some random purchases of items by users
    PROBABILITY_PURCHASED = 0.1
    NUM = 100
    purchase_requests = []

    for user_id in ["user-%s" % i for i in range(NUM) ]:
      for item_id in ["item-%s" % i for i in range(NUM) ]:
        if random.random() < PROBABILITY_PURCHASED:

          request = AddPurchase(user_id, item_id, cascade_create=True)
          purchase_requests.append(request)

    try:
        # Send the data to Recombee, use Batch for faster processing of larger data
        print('Send purchases')
        client.send(Batch(purchase_requests))

        # Get recommendations for user 'user-25'
        response = client.send(RecommendItemsToUser('user-25', 5))
        print("Recommended items: %s" % response)

        # User scrolled down - get next 3 recommended items
        response = client.send(RecommendNextItems(response['recommId'], 3))
        print("Next recommended items: %s" % response)

    except APIException as e:
        print(e)

---------------------
Using property values
---------------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.api_requests import AddItemProperty, SetItemValues, AddPurchase
    from recombee_api_client.api_requests import RecommendItemsToItem, SearchItems, Batch, ResetDatabase
    import random

    NUM = 100
    PROBABILITY_PURCHASED = 0.1

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.AP_SE)

    # Clear the entire database
    client.send(ResetDatabase())

    # We will use computers as items in this example
    # Computers have four properties 
    #   - price (floating point number)
    #   - number of processor cores (integer number)
    #   - description (string)
    #   - image (url of computer's photo)

    # Add properties of items
    client.send(AddItemProperty('price', 'double'))
    client.send(AddItemProperty('num-cores', 'int'))
    client.send(AddItemProperty('description', 'string'))
    client.send(AddItemProperty('image', 'image'))

    # Prepare requests for setting a catalog of computers
    requests = [SetItemValues(
        "computer-%s" % i, #itemId
        #values:
        { 
          'price': random.uniform(500, 2000),
          'num-cores': random.randrange(1,9),
          'description': 'Great computer',
          'image': 'http://examplesite.com/products/computer-%s.jpg' % i
        },
        cascade_create=True   # Use cascadeCreate for creating item
                              # with given itemId if it doesn't exist
      ) for i in range(NUM)]

    # Send catalog to the recommender system
    client.send(Batch(requests))

    # Prepare some purchases of items by users
    requests = []
    items = ["computer-%s" % i for i in range(NUM)]
    users = ["user-%s" % i for i in range(NUM)]

    for item_id in items:
        #Use cascadeCreate to create unexisting users
        purchasing_users = [user_id for user_id in users if random.random() < PROBABILITY_PURCHASED]
        requests += [AddPurchase(user_id, item_id, cascade_create=True) for user_id in purchasing_users]

    # Send purchases to the recommender system
    client.send(Batch(requests))

    # Get 5 recommendations for user-42, who is currently viewing computer-6
    # Recommend only computers that have at least 3 cores
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'num-cores'>=3")
    )
    print("Recommended items with at least 3 processor cores: %s" % recommended)

    # Recommend only items that are more expensive then currently viewed item (up-sell)
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
    print("Recommended up-sell items: %s" % recommended)

    # Filters, boosters and other settings can be also set in the Admin UI (admin.recombee.com)
    # when scenario is specified
    recommended = client.send(
      RecommendItemsToItem('computer-6', 'user-42', 5, scenario='product_detail')
      )

    # Perform personalized full-text search with a user's search query (e.g. 'computers').
    matches = client.send(SearchItems('user-42', 'computers', 5, scenario='search_top'))
    print("Matched items: %s" % matches)

------------------
Exception handling
------------------

For the sake of brevity, the above examples omit exception handling. However, various exceptions can occur while processing request, for example because of adding an already existing item, submitting interaction of nonexistent user or because of timeout.

We are doing our best to provide the fastest and most reliable service, but production-level applications must implement a fallback solution since errors can always happen. The fallback might be, for example, showing the most popular items from the current category, or not displaying recommendations at all.

Example:

.. code-block:: python

  from recombee_api_client.exceptions import *

  try:
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
  except ResponseException as e:
    #Handle errorneous request => use fallback
  except ApiTimeoutException as e:
    #Handle timeout => use fallback
  except APIException as e:
    #APIException is parent of both ResponseException and ApiTimeoutException

---

## Installation

=============

Install the client with pip:

.. code-block:: bash

    $ pip install recombee-api-client

========
Examples
========

-------------
Basic example
-------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.exceptions import APIException
    from recombee_api_client.api_requests import *
    import random

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.US_WEST)

    #Generate some random purchases of items by users
    PROBABILITY_PURCHASED = 0.1
    NUM = 100
    purchase_requests = []

    for user_id in ["user-%s" % i for i in range(NUM) ]:
      for item_id in ["item-%s" % i for i in range(NUM) ]:
        if random.random() < PROBABILITY_PURCHASED:

          request = AddPurchase(user_id, item_id, cascade_create=True)
          purchase_requests.append(request)

    try:
        # Send the data to Recombee, use Batch for faster processing of larger data
        print('Send purchases')
        client.send(Batch(purchase_requests))

        # Get recommendations for user 'user-25'
        response = client.send(RecommendItemsToUser('user-25', 5))
        print("Recommended items: %s" % response)

        # User scrolled down - get next 3 recommended items
        response = client.send(RecommendNextItems(response['recommId'], 3))
        print("Next recommended items: %s" % response)

    except APIException as e:
        print(e)

---------------------
Using property values
---------------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.api_requests import AddItemProperty, SetItemValues, AddPurchase
    from recombee_api_client.api_requests import RecommendItemsToItem, SearchItems, Batch, ResetDatabase
    import random

    NUM = 100
    PROBABILITY_PURCHASED = 0.1

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.AP_SE)

    # Clear the entire database
    client.send(ResetDatabase())

    # We will use computers as items in this example
    # Computers have four properties 
    #   - price (floating point number)
    #   - number of processor cores (integer number)
    #   - description (string)
    #   - image (url of computer's photo)

    # Add properties of items
    client.send(AddItemProperty('price', 'double'))
    client.send(AddItemProperty('num-cores', 'int'))
    client.send(AddItemProperty('description', 'string'))
    client.send(AddItemProperty('image', 'image'))

    # Prepare requests for setting a catalog of computers
    requests = [SetItemValues(
        "computer-%s" % i, #itemId
        #values:
        { 
          'price': random.uniform(500, 2000),
          'num-cores': random.randrange(1,9),
          'description': 'Great computer',
          'image': 'http://examplesite.com/products/computer-%s.jpg' % i
        },
        cascade_create=True   # Use cascadeCreate for creating item
                              # with given itemId if it doesn't exist
      ) for i in range(NUM)]

    # Send catalog to the recommender system
    client.send(Batch(requests))

    # Prepare some purchases of items by users
    requests = []
    items = ["computer-%s" % i for i in range(NUM)]
    users = ["user-%s" % i for i in range(NUM)]

    for item_id in items:
        #Use cascadeCreate to create unexisting users
        purchasing_users = [user_id for user_id in users if random.random() < PROBABILITY_PURCHASED]
        requests += [AddPurchase(user_id, item_id, cascade_create=True) for user_id in purchasing_users]

    # Send purchases to the recommender system
    client.send(Batch(requests))

    # Get 5 recommendations for user-42, who is currently viewing computer-6
    # Recommend only computers that have at least 3 cores
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'num-cores'>=3")
    )
    print("Recommended items with at least 3 processor cores: %s" % recommended)

    # Recommend only items that are more expensive then currently viewed item (up-sell)
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
    print("Recommended up-sell items: %s" % recommended)

    # Filters, boosters and other settings can be also set in the Admin UI (admin.recombee.com)
    # when scenario is specified
    recommended = client.send(
      RecommendItemsToItem('computer-6', 'user-42', 5, scenario='product_detail')
      )

    # Perform personalized full-text search with a user's search query (e.g. 'computers').
    matches = client.send(SearchItems('user-42', 'computers', 5, scenario='search_top'))
    print("Matched items: %s" % matches)

------------------
Exception handling
------------------

For the sake of brevity, the above examples omit exception handling. However, various exceptions can occur while processing request, for example because of adding an already existing item, submitting interaction of nonexistent user or because of timeout.

We are doing our best to provide the fastest and most reliable service, but production-level applications must implement a fallback solution since errors can always happen. The fallback might be, for example, showing the most popular items from the current category, or not displaying recommendations at all.

Example:

.. code-block:: python

  from recombee_api_client.exceptions import *

  try:
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
  except ResponseException as e:
    #Handle errorneous request => use fallback
  except ApiTimeoutException as e:
    #Handle timeout => use fallback
  except APIException as e:
    #APIException is parent of both ResponseException and ApiTimeoutException

## Usage

If you don't have an account at Recombee yet, you can create a free account `here <https://www.recombee.com/>`_.

Documentation of the API can be found at `docs.recombee.com <https://docs.recombee.com/>`_.

=============
Installation
=============

Install the client with pip:

.. code-block:: bash

    $ pip install recombee-api-client

========
Examples
========

-------------
Basic example
-------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.exceptions import APIException
    from recombee_api_client.api_requests import *
    import random

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.US_WEST)

    #Generate some random purchases of items by users
    PROBABILITY_PURCHASED = 0.1
    NUM = 100
    purchase_requests = []

    for user_id in ["user-%s" % i for i in range(NUM) ]:
      for item_id in ["item-%s" % i for i in range(NUM) ]:
        if random.random() < PROBABILITY_PURCHASED:

          request = AddPurchase(user_id, item_id, cascade_create=True)
          purchase_requests.append(request)

    try:
        # Send the data to Recombee, use Batch for faster processing of larger data
        print('Send purchases')
        client.send(Batch(purchase_requests))

        # Get recommendations for user 'user-25'
        response = client.send(RecommendItemsToUser('user-25', 5))
        print("Recommended items: %s" % response)

        # User scrolled down - get next 3 recommended items
        response = client.send(RecommendNextItems(response['recommId'], 3))
        print("Next recommended items: %s" % response)

    except APIException as e:
        print(e)

---------------------
Using property values
---------------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.api_requests import AddItemProperty, SetItemValues, AddPurchase
    from recombee_api_client.api_requests import RecommendItemsToItem, SearchItems, Batch, ResetDatabase
    import random

    NUM = 100
    PROBABILITY_PURCHASED = 0.1

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.AP_SE)

    # Clear the entire database
    client.send(ResetDatabase())

    # We will use computers as items in this example
    # Computers have four properties 
    #   - price (floating point number)
    #   - number of processor cores (integer number)
    #   - description (string)
    #   - image (url of computer's photo)

    # Add properties of items
    client.send(AddItemProperty('price', 'double'))
    client.send(AddItemProperty('num-cores', 'int'))
    client.send(AddItemProperty('description', 'string'))
    client.send(AddItemProperty('image', 'image'))

    # Prepare requests for setting a catalog of computers
    requests = [SetItemValues(
        "computer-%s" % i, #itemId
        #values:
        { 
          'price': random.uniform(500, 2000),
          'num-cores': random.randrange(1,9),
          'description': 'Great computer',
          'image': 'http://examplesite.com/products/computer-%s.jpg' % i
        },
        cascade_create=True   # Use cascadeCreate for creating item
                              # with given itemId if it doesn't exist
      ) for i in range(NUM)]

    # Send catalog to the recommender system
    client.send(Batch(requests))

    # Prepare some purchases of items by users
    requests = []
    items = ["computer-%s" % i for i in range(NUM)]
    users = ["user-%s" % i for i in range(NUM)]

    for item_id in items:
        #Use cascadeCreate to create unexisting users
        purchasing_users = [user_id for user_id in users if random.random() < PROBABILITY_PURCHASED]
        requests += [AddPurchase(user_id, item_id, cascade_create=True) for user_id in purchasing_users]

    # Send purchases to the recommender system
    client.send(Batch(requests))

    # Get 5 recommendations for user-42, who is currently viewing computer-6
    # Recommend only computers that have at least 3 cores
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'num-cores'>=3")
    )
    print("Recommended items with at least 3 processor cores: %s" % recommended)

    # Recommend only items that are more expensive then currently viewed item (up-sell)
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
    print("Recommended up-sell items: %s" % recommended)

    # Filters, boosters and other settings can be also set in the Admin UI (admin.recombee.com)
    # when scenario is specified
    recommended = client.send(
      RecommendItemsToItem('computer-6', 'user-42', 5, scenario='product_detail')
      )

    # Perform personalized full-text search with a user's search query (e.g. 'computers').
    matches = client.send(SearchItems('user-42', 'computers', 5, scenario='search_top'))
    print("Matched items: %s" % matches)

------------------
Exception handling
------------------

For the sake of brevity, the above examples omit exception handling. However, various exceptions can occur while processing request, for example because of adding an already existing item, submitting interaction of nonexistent user or because of timeout.

We are doing our best to provide the fastest and most reliable service, but production-level applications must implement a fallback solution since errors can always happen. The fallback might be, for example, showing the most popular items from the current category, or not displaying recommendations at all.

Example:

.. code-block:: python

  from recombee_api_client.exceptions import *

  try:
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
  except ResponseException as e:
    #Handle errorneous request => use fallback
  except ApiTimeoutException as e:
    #Handle timeout => use fallback
  except APIException as e:
    #APIException is parent of both ResponseException and ApiTimeoutException

## API

*****************

A Python 3 client for easy use of the `Recombee <https://www.recombee.com/>`_  recommendation API.

If you don't have an account at Recombee yet, you can create a free account `here <https://www.recombee.com/>`_.

Documentation of the API can be found at `docs.recombee.com <https://docs.recombee.com/>`_.

=============
Installation
=============

Install the client with pip:

.. code-block:: bash

    $ pip install recombee-api-client

========
Examples
========

-------------
Basic example
-------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.exceptions import APIException
    from recombee_api_client.api_requests import *
    import random

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.US_WEST)

    #Generate some random purchases of items by users
    PROBABILITY_PURCHASED = 0.1
    NUM = 100
    purchase_requests = []

    for user_id in ["user-%s" % i for i in range(NUM) ]:
      for item_id in ["item-%s" % i for i in range(NUM) ]:
        if random.random() < PROBABILITY_PURCHASED:

          request = AddPurchase(user_id, item_id, cascade_create=True)
          purchase_requests.append(request)

    try:
        # Send the data to Recombee, use Batch for faster processing of larger data
        print('Send purchases')
        client.send(Batch(purchase_requests))

        # Get recommendations for user 'user-25'
        response = client.send(RecommendItemsToUser('user-25', 5))
        print("Recommended items: %s" % response)

        # User scrolled down - get next 3 recommended items
        response = client.send(RecommendNextItems(response['recommId'], 3))
        print("Next recommended items: %s" % response)

    except APIException as e:
        print(e)

---------------------
Using property values
---------------------

.. code-block:: python

    from recombee_api_client.api_client import RecombeeClient, Region
    from recombee_api_client.api_requests import AddItemProperty, SetItemValues, AddPurchase
    from recombee_api_client.api_requests import RecommendItemsToItem, SearchItems, Batch, ResetDatabase
    import random

    NUM = 100
    PROBABILITY_PURCHASED = 0.1

    client = RecombeeClient('--my-database-id--', '--db-private-token--', region=Region.AP_SE)

    # Clear the entire database
    client.send(ResetDatabase())

    # We will use computers as items in this example
    # Computers have four properties 
    #   - price (floating point number)
    #   - number of processor cores (integer number)
    #   - description (string)
    #   - image (url of computer's photo)

    # Add properties of items
    client.send(AddItemProperty('price', 'double'))
    client.send(AddItemProperty('num-cores', 'int'))
    client.send(AddItemProperty('description', 'string'))
    client.send(AddItemProperty('image', 'image'))

    # Prepare requests for setting a catalog of computers
    requests = [SetItemValues(
        "computer-%s" % i, #itemId
        #values:
        { 
          'price': random.uniform(500, 2000),
          'num-cores': random.randrange(1,9),
          'description': 'Great computer',
          'image': 'http://examplesite.com/products/computer-%s.jpg' % i
        },
        cascade_create=True   # Use cascadeCreate for creating item
                              # with given itemId if it doesn't exist
      ) for i in range(NUM)]

    # Send catalog to the recommender system
    client.send(Batch(requests))

    # Prepare some purchases of items by users
    requests = []
    items = ["computer-%s" % i for i in range(NUM)]
    users = ["user-%s" % i for i in range(NUM)]

    for item_id in items:
        #Use cascadeCreate to create unexisting users
        purchasing_users = [user_id for user_id in users if random.random() < PROBABILITY_PURCHASED]
        requests += [AddPurchase(user_id, item_id, cascade_create=True) for user_id in purchasing_users]

    # Send purchases to the recommender system
    client.send(Batch(requests))

    # Get 5 recommendations for user-42, who is currently viewing computer-6
    # Recommend only computers that have at least 3 cores
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'num-cores'>=3")
    )
    print("Recommended items with at least 3 processor cores: %s" % recommended)

    # Recommend only items that are more expensive then currently viewed item (up-sell)
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
    print("Recommended up-sell items: %s" % recommended)

    # Filters, boosters and other settings can be also set in the Admin UI (admin.recombee.com)
    # when scenario is specified
    recommended = client.send(
      RecommendItemsToItem('computer-6', 'user-42', 5, scenario='product_detail')
      )

    # Perform personalized full-text search with a user's search query (e.g. 'computers').
    matches = client.send(SearchItems('user-42', 'computers', 5, scenario='search_top'))
    print("Matched items: %s" % matches)

------------------
Exception handling
------------------

For the sake of brevity, the above examples omit exception handling. However, various exceptions can occur while processing request, for example because of adding an already existing item, submitting interaction of nonexistent user or because of timeout.

We are doing our best to provide the fastest and most reliable service, but production-level applications must implement a fallback solution since errors can always happen. The fallback might be, for example, showing the most popular items from the current category, or not displaying recommendations at all.

Example:

.. code-block:: python

  from recombee_api_client.exceptions import *

  try:
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
  except ResponseException as e:
    #Handle errorneous request => use fallback
  except ApiTimeoutException as e:
    #Handle timeout => use fallback
  except APIException as e:
    #APIException is parent of both ResponseException and ApiTimeoutException

## Dependencies

| Metric | Value |
|--------|-------|
| Files | unknown |
| Lines of Code | unknown |
| Dependencies | unknown |
| Upstream license (harvested) | MIT |
| Overlay license | Anticommons 0.1.0 |

Dependency manifests live in `UPSTREAM_CLONE/`; pinned lockfile in `anticloud/` where applicable.

## Configuration

# when scenario is specified
    recommended = client.send(
      RecommendItemsToItem('computer-6', 'user-42', 5, scenario='product_detail')
      )

    # Perform personalized full-text search with a user's search query (e.g. 'computers').
    matches = client.send(SearchItems('user-42', 'computers', 5, scenario='search_top'))
    print("Matched items: %s" % matches)

------------------
Exception handling
------------------

For the sake of brevity, the above examples omit exception handling. However, various exceptions can occur while processing request, for example because of adding an already existing item, submitting interaction of nonexistent user or because of timeout.

We are doing our best to provide the fastest and most reliable service, but production-level applications must implement a fallback solution since errors can always happen. The fallback might be, for example, showing the most popular items from the current category, or not displaying recommendations at all.

Example:

.. code-block:: python

  from recombee_api_client.exceptions import *

  try:
    recommended = client.send(
        RecommendItemsToItem('computer-6', 'user-42', 5, filter="'price' > context_item[\"price\"]")
    )
  except ResponseException as e:
    #Handle errorneous request => use fallback
  except ApiTimeoutException as e:
    #Handle timeout => use fallback
  except APIException as e:
    #APIException is parent of both ResponseException and ApiTimeoutException

## Contributing

Fork the project, create a feature branch, run the test suite, and open a pull request against upstream.

## License

Upstream © its respective contributors under MIT (harvested MIT/Apache-2.0/BSD source; see `UPSTREAM_CLONE/LICENSE`). This packaging overlay is licensed under Anticommons 0.1.0.

## Upstream

- **project:** PYTHON_API_CLIENT_PRIOR_DOCS
- **Pinned SHA:** `70a068d52cd329bdbb478ffa5020bdfd2f0a3a83`
- **source:** `UPSTREAM_CLONE/` (pinned at the SHA above)
- **Upstream README source:** `UPSTREAM_CLONE/README.rst`

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`10cd5a96a3ee4840543ec2e2ce243461b8aa941682c42de8a60e57a6186057e6`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

