---
toc: true
layout: post
comments: true
description: ""
categories: [software-engineering, python, pytest. testing]
title: Python Testing with Pytest notes: CHAPTER 2 "Writing Test Functions"
---

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

# Marking Test Functions
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

# Skipping Tests
- Pytest has a few helpful builtin markers like `skip` and `skipif` for skipping the test
- We can use it to skip test that we don't want to run.
  ```python
  @pytest.mark.skip(reason='misunderstood the API')
  def test_unique_id_1():
  """Calling unique_id() twice should return different numbers."""
  id_1 = tasks.unique_id()
  id_2 = tasks.unique_id()
  assert id_1 != id_2
  ```
- Sometime we skip the test unless some conditions are met.
  ```python
  @pytest.mark.skipif(tasks.__version__ < '0.2.0', #expression can be any valid python expression
                      reason='not supported until version 0.2.0')
  def test_unique_id_1():
  """Calling unique_id() twice should return different numbers."""
  id_1 = tasks.unique_id()
  id_2 = tasks.unique_id()
  assert id_1 != id_2
  ```
- Include reason of skip of the test. We can see the reason for skipping of test with `-rs` flag.
  ```python
  $ pytest -rs test_unique_id_3.py
  ```
  
# Marking Tests as Expecting to Fail
- Use `xfail` builtin marker to mark test to run but it's expected to fail.
- In the report, `x` is for `XFAIL`, meaning "expected to fail", `X` is for `XPASS`, meaning "expected to fail but passed" 
- We can configure pytest to report the tests that pass but were marked with `xfail` as FAIL.  Just add this in *pytest.ini* file:
  ```config
  [pytest]
  xfail_strict=true
  ```
