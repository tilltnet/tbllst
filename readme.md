# tbllst (pronounced "tibble list")

Working with (long) lists of  dataframes can be tedious. tbllst provides an infrastructure,
that makes it easy to use the tidyverse grammar, to work with lists housing data.frames.

# In early development! 

See issue tracker for discussions on implementation of main features.

# Features
- create/verify tbllst object structure()
  - what to do with non-data.frame list elements? (delete/ move to attributes/ ...?)
  - use `list()` for list creation, in order to always produce named lists.
  - tibble_list/ tbl_lst/ tbllst(+)
- can be used with data.frames, tibble and data.table(?). 
  - convert data.frames to tbl?
  - have a separate data class for dataframes `tbldf` (?)
- use the activate() concept, primed by Thomas Lin Pedersen with the tidygraph package, to switch between the dataframes in a list and manipulate a specific one.
- Apply operations to groups, different ideas:
  - activate_all(), activate_at(), activate_if(), activate_where()
  - across()
  - switch with, `mode=` argument in `activate()`, default to `hierachical` if id dependencies are defined
    - modes could be
      - `serial` operations are executed on all active dataframes. Evaluate in parallel if `future` installed and `plan()` is set to anything but `sequential`.
      - `hierachical` operations especially `filter()` are executed according to id dependencies (like filter edges, when nodes are deleted). How to define dependencies? (attributes object level/ attributes tbl level (+)/ extra config element/ global options/ ... ?
    - if hierarchy definitions are found those could automaticall be taken into regard   
  

# Helper functions / specific behavior

- has() checks if a dataframe has a certain variable
- trim() manually trigger resolution of id incongruences
  - `mode` only active or for all defined id dependencies
- `bind_rows()` method for `tbllst` based on `rdimensions::bind_results()`
- `dplyr::*_join()` are applied to all active elements. If simple dependencies exist (i.e. only one variable depends on a variable in another data frame) these are automatically used for `by=`. (`purrr::reduce()`)

  