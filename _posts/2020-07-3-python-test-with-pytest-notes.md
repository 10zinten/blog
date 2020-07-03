---
toc: true
layout: post
comments: true
description: Important notes from Python Testing with Pytest book
categories: [software-engineering, python]
title: Python Testing with Pytest notes
---

# CHAPTER 2: Writing Test Functions
# Expecting Exceptions
- Make sure a function (especially API function) raises expected exception.
- Always raise exception from API function if desirable type is not passed.
- Simple check for type of exception raised example.
  ```python
  def test_add_raises():
      """add() should raise an exception with wrong type param."""
      with pytest.raises(TypeError):
          tasks.add(task='not a Task object')
   ```
   - Non `Task` object is added to tasks list. `add` method should raise `TypeError` exception

- Sometime we may need to check for exception parameters.
- This let us look at exception more closely.
  ```python
  def test_start_tasks_db_raises():
    """Make sure unsupported db raises an exception."""
    with pytest.raises(ValueError) as excinfo:
        tasks.start_tasks_db('some/great/path', 'mysql')
    exception_msg = excinfo.value.args[0]
    assert exception_msg == "db_type must be a 'tiny' or 'mongo'"
  ```
  - Not only *db_type* should be `str` (passed `mysql` *db_type*), it should be either `tiny` or `mongo`.

## Marking Test Functions
- pytest provides a cool mechanism to let us mark the test functions
- Useful for marking subset of our tests as "smoke test".
- **Smoke test** let us get a sense for whether or not there is some major break in the system. It's by convention not all-inclusive of tests.
- **Smoke test** let us get a sense for whether or not there is some major break in the system. It's by convention not all-inclusive of tests.
  ```python
  @pytest.mark.smoke
  def test_list_raises():
      """list() should raise an exception with wrong type param."""
      with pytest.raises(TypeError):
          tasks.list_tasks(owner=123)


  @pytest.mark.get
  @pytest.mark.smoke
  def test_get_raises():
      """get() should raise an exception with wrong type param."""
      with pytest.raises(TypeError):
          tasks.get(task_id='123')
  ```
- Run the smoke test with `-m` option
  ```bash
  $ pytest -m "smoke" test_api_exceptions.py
  $ pytest -v -m 'smoke and not get' test_api_exceptions.py
  ```
