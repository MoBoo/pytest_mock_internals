# pytest_mock_internals
1. Pytest creates fixtures: `_pytest/fixtures:886` and`pytest_mock/plugin:443`
2. Creates mocker object with a custom patch function (`pytest_mock/plugin:55`) as a wrapper around `mock.patch`
3. `mocker.patch()` calls `p=mock.patch()` (`pytest_mock/plugin:417`) and eventually `yields p.start()` (the mocked object) to test function (`pytest_mock/plugin:228`)
4. finally pytest.fixture calls `next()` (_pytest/fixtures:910) on mocker fixture. mocker calls `p.stop()` (`pytest_mock/plugin:440+112`) to clean up any mocked objects.

Minimal starting point: `my_test.py`
```python
from pytest import fixture
from pytest_mock import mocker
import os

def test_mkdir(mocker):
    print("foo")
    mock_os_mkdir = mocker.patch("my_test.os.mkdir")
    mock_os_mkdir.return_value = "foo"

    os.mkdir("asd")
```
