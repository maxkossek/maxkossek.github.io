---
layout: post
title: The Problem of Circularity on Review Sites
date: 2025-08-04
categories: [society]
tags: [algorithms, consumerism, marketing]
author: "Max Kossek"
description: A short article about what you can tell about a person based on their use of review sites and deviation of ratings from the average.
sitemap:
    lastmod: 2025-08-04
---

Today, the first thing most people do when confronted with a decision is to consult review sites. This aggregate of previous reviews influences your decision then to decide whether to consider the item, location, or experience. It has been applied in many areas to review: pieces of art (GoodReads, RateYourMusic, Letterboxd, IMDb), products (Amazon, eBay), gig worker performance (Uber, Lyft), physical locations (Google, Yelp), hospitality (TripAdvisor, AirBnB).

But ratings on review sites don't just tell us things about the reviewed, but also the reviewer. One can measure how conventional-minded or independent-minded a person is by how far their ratings deviate from the average ratings on review sites. For example, does a person rate items similar to other people on review sites? If yes, they tend to be more conventional-minded and should be weighted less on review sites. I've noticed with myself that seeing the ratings of a products or piece of art have influenced my perception of its quality. For example, I might quit watching--or not watch at all--a movie with a bad IMDb score earlier than a movie with a good score.

This is the problem of circularity of review sites. A piece of art that has been rated well in the past is more likely to be given a chance--as well as more leeway--from users. Amazon customers are unlikely to even consider purchasing products with a rating lower than 4 stars. In fact, Amazon removed all filter for Customer Reviews except for 4 stars and up.

Because of path dependence, reviewed items that get good reviews early on are likely to become popular, or have other people review the item favorably. This can be make-or-break for a business, piece of art, or gig worker. Often, the only way to get rid of a bad rating is to "start over", that is, going bankrupt, recreating an account, or rebranding the product. One way to resolve this is to make ratings time-dependent, such that recent ratings are weighted more heavily than older reviews. But this is not perfect because individuals are still subtly influenced by the current rating. Another method is to hide the rating, and then allow for some perturbations in the recommendation algorithm. This is what most recommendation algorithm-based sites (such as YouTube, Netflix) rather than ratings-based sites rely on.

Different industries and firms will deal with this circularity problem differently. In general, this circularity favors customers in the short-term, but may harm them in the long-term. For the owners of the platforms, reviews "discipline" the products to focus on providing good quality and service. Reviews can be seen as a form of non-monetary gratuity (tip). But the accumulative nature of reviews on many platforms may also create winner-takes-all effects that lead to monopoly-like effects. This will again vary by platform and could be easily mitigated by algorithm changes.
