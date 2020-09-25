# beforematch event

_These changes will likely go into the
[find-in-page](https://html.spec.whatwg.org/multipage/interaction.html#find-in-page)
section of the HTML spec_

When a new active match is set, either by advancing through the match list or
due to a new find-in-page request, a `beforematch` event is fired on a node
identified by the active match [range
start](https://dom.spec.whatwg.org/#concept-range-start). This process follows
the following algorithm:

1. Identify the candidate match _M_ which will become the new active match. _M_'s DOM
range must not be [collapsed](https://dom.spec.whatwg.org/#range-collapsed).

2. At the next rendering opportunity, specifically step 12 of [Update the
rendering](https://html.spec.whatwg.org/#rendering-opportunity) step:

      2.1. If the range that constitutes match _M_ is
        [collapsed](https://dom.spec.whatwg.org/#range-collapsed), restart the
        algorithm at Step 1 with the match following _M_ as the new candidate
        match.

      2.2. Fire a `beforematch` event on an element identified by _M_'s range start node.

3. At the next rendering opportunity (the _next_ Step 12 of [Update the
rendering](https://html.spec.whatwg.org/#rendering-opportunity)):

      3.1 If the range which constitutes match _M_ is
      [collapsed](https://dom.spec.whatwg.org/#range-collapsed), or the range
      start node is not shown to the user (e.g. it is inside a
      `content-visibility: hidden-matchable` subtree), restart the algorithm at
      Step 1 with with match following _M_ as the new candidate match.

      3.2 Scroll the range start node of match _M_ into view, following the same
      algorithm as if
      [`scrollIntoView()`](https://drafts.csswg.org/cssom-view/#dom-element-scrollintoview)
      was invoked on that node.

# content-visibility: hidden-matchable

_These changes will likely go into the
[css-contain-2](https://www.w3.org/TR/css-contain-2/#content-visibility)
module_.

'hidden-matchable': 

  This value behaves very similarly to 'hidden'.
  That is, the element [skips its contents](https://www.w3.org/TR/css-contain-2/#skips-its-contents).

  The skipped contents must not be accessible to user-agent features such as
  tab-order navigation, nor be selectable or focusable.

  However, unlike 'hidden', the skipped contents must be accessible to the
  find-in-page algorithm in order to allow the `beforematch` event to fire.
  (TODO: reference the find-in-page beforematch spec)