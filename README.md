# Rank Ordering Items
In a so-called “rank order item set,” or as I prefer, an “item ordering,” a user's responses are forced into an even distribution across all possible response options. It may be interesting to note that it is generally the case with any kind of item set that the response options are the same for every item in the set. This is an important fact when it comes to item ordering steps, as we shall see.

In a sokanu assessment, item ordering steps are provided basically the same data as the more common item sequence step. The sokanu engine api distinguishes these two steps with the type attribute, and they are otherwise exactly the same.

    // An item sequence step provided by sokanu engine and slightly trimmed
    {
        "id": 1,
        "type": "sequential_item_set",
        "title": "Hollands Codes",
        "order": 6,
        "meta": {
            "item_count": 44
            ...
        }
    }

    // An item ordering step provided by sokanu engine and slightly trimmed
    {
        "id": 2,
        "type": "rank_order_item_set",
        "title": "Must Haves",
        "order": 8,
        "meta": {
            "item_count": 20
            ...
        }
    }

Comparison between item ordering and item sequence data. Aside from `type`, there is no difference.

## Creating A Distribution
In my best estimation, the first step towards an item ordering implementation is to map out the distribution of responses. With this map of response distribution you can constrain interactions towards the desired outcome. The two pieces of data needed to do this are:
1. the number of items to be ordered
2. the number of response options
It's almost trivial to map out an even distribution, given any number of items and any number of response options. Things become more difficult, though, when the number of items you're given doesn't evenly divide by the number of response options you're given. Consider this example method, which will divide evenly if possible, but frontload the distribution with any remainder.

    function createDistribution (itemCount, responseOptions) {
        var distribution = [];
        responseOptions.forEach(function (responseOption) {
            distribution.push(Math.floor(itemCount / responseOptions.length));
        });
        var remainder = itemCount % responseOptions.length;
        while (remainder > 0) {
            var i = i || 0;
            distribution[i] += 1;
            remainder--;
            i++;
        }
        return distribution;
    }

    // Logs [4, 4, 4, 4]
    console.log(createDistribution(20, [{},{},{},{},{}]));

    // Logs [3, 3, 2, 2, 2]
    console.log(createDistribution(12, [{},{},{},{},{}]));

Example distribution creation method for an item ordering step. http://jsfiddle.net/8G3CL/1/

♥ from Jørgen, Ryan and the rest of the sokanu team