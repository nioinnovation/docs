# Testing your block

nio Blocks are not meant to be run as stand-alone Python modules, so testing can be a challenging process. The nio Platform provides a couple of tools and offers best practices to make your testing easier.

---

## <span class="allow-caps">NIOBlockTestCase</span>

When building your tests, use the `NIOBlockTestCase`. The test case extends the `unittest.TestCase` and uses the same infrastructure as the base class. However, `NIOBlockTestCase` also provides a number of helpful methods for testing blocks. Here's a real-world example from our internal block repositories:

```python
from nio.testing.block_test_case import NIOBlockTestCase
from nio.signal.base import Signal
from ..your_block import YourBlock

class TestYourBlock(NIOBlockTestCase):

	 def test_some_feature(self):
		 blk = YourBlock()
		 self.configure_block(blk, {
			 'int_prop': 23,
			 'obj_prop': {
				 'p1': 'foo',
				 'p2': 'bar'
			 }
		 })
		 blk.start()
		 blk.process_signals([Signal()])
		 blk.stop()
		 # your assertions here…

 ```

---

## setUp/tearDown

Just like the `unittest.TestCase`, nio supports the setup/teardown pattern. This is a great place to do any initialization and/or cleanup that will be required across every test. Do not repeat yourself! Be aware, though, that the block provides crucial initialization and finalization in `NIOBlockTestCase.setUp/tearDown`. If you override either of these methods, you need to call the method in the parent class at the top of your method.

---

## Helper methods

- **configure_block(block, block_properties)**<br>The process of configuring and initializing blocks manually is somewhat nuanced (and not something we want you to worry about). We provide this method to configure your block instance semi-automatically. Just pass the block object itself and a dictionary containing any block properties you want to configure (and the associated values).
- **last_signal_notified(output_id)**<br>This method returns the last signal that was emitted from a particular output. If an **output_id** is not specified, it will return the last signal emitted from any output on the block.

---

## Assertion methods

- **assert_block_status(block, status)**<br>Assert that the `block` is in `status`, which is one of `""`, `"ok"`, `"warning"`, or `"error"`.
- **assert_last_signal_notified(signal, output_id=DEFAULT_TERMINAL)**<br>Assert that the last signal notified from `output_id` matches `signal`. See also `assert_signal_notified`.
- **assert_last_signal_list_notified(signal_list, output_id=DEFAULT_TERMINAL)**<br>Assert that the last signal list notified from `output_id` matches `signal_list`. See also `assert_signal_list_notified`.
- **assert_num_mgmt_signals_notified(num, block=None)**<br>Compare the total number of management signals notified from `block` to `num`, and fail if not equal. If `block` is not given (defaulting to `None`) then all blocks will be considered in the comparison. Note that during block testing there is generally only one block configured, so `block` generally does not need to be specified when calling this method.
- **assert_num_signals_notified(num, block=None)**<br>Compare the total number of signals notified from `block` to `num`, and fail if not equal. If `block` is not given (defaulting to `None`) then all blocks will be considered in the comparison. Note that during block testing there is generally only one block configured, so `block` generally does not need to be specified when calling this method.
- **assert_signal_notified(signal, position=None, output_id=DEFAULT_TERMINAL)**<br>Check that `signal` is in the list of all signals notified from `output_id`, and fail if an exact match is not found. Optionally pass `position` to assert the index of `signal`. If your block has more than one output terminal, pass a `terminal_id` to `output_id`.
- **assert_signal_list_notified(signal_list, position=None, output_id=DEFAULT_TERMINAL)**<br>Check that `signal_list` is in the list of all signal lists notified from `output_id`, and fail if an exact match is not found. Optionally pass `position` to assert the index of `signal`. If your block has more than one output terminal, pass a `terminal_id` to `output_id`.

---

## Overridable methods

- **get_test_modules()**<br>By default, `NIOBlockTestCase` automatically initializes the logging, threading, scheduler, and security modules. However, you can customize this by overriding this method and returning a list of strings corresponding to the particular modules you want to initialize.
    * logging
    * threading
    * scheduler
    * security
    * communication
    * persistence
    * web
- **signals_notified(signals, output_id)**<br>This method gets called every time signals are emitted in your tests. If you'd like to record something in the test case, trigger an event, or perform some aggregation when that happens, override this method.

```python
def setUp(self):
    super().setUp()
    self.signals = defaultdict(list)

def signals_notified(self, signals, output_id):
    super().signals_notfied(signals, output_id)
	self.signals[output_id].extend(signals)
```

- **management_signal_notified(signals)**<br>This method gets called every time signals are emitted in your tests. If you'd like to record something in the test case, trigger an event, or perform some aggregation when that happens, override this method.

```python
def setUp(self):
    super().setUp()
    self.signals = defaultdict(list)

def signals_notified(self, signals, output_id):
    super().signals_notfied(signals, output_id)
	self.signals[output_id].extend(signals)
```

---

## Events

Blocks are not required to behave synchronously, and sometimes you may want to wait for an event (after instantiating or configuring a block) before proceeding with the tests. Rather than sleeping for a prescribed amount of time (asynchronous processes can be fickle and unpredictable from machine to machine). You can extend  the block and add one of Python's Event objects to signify readiness.

```python
class EventBlock(YourBlock):
	def __init__(self, event):
		super().__init__()
		self.e = event

	def configure(self, context):
		super().configure(context)
		self.e.set()
```

Now, instead of instantiating `YourBlock` in the tests, instantiate `EventBlock` passing an instance of `Event` to its constructor.

```python
from threading import Event
e = Event()
blk = EventBlock(e)
self.configure_block(blk, {'p1': 23})
e.wait(2)
```

Using the _EventBlock_, your test will wait until  `YourBlock.configure` returns control to the method on the child class. Your test will never proceed until `EventBlock.e` is set.

---

## Mocking

Patching and mocking are extremely useful concepts in software verification; this is especially relevant when the modules in question interact with external resources (such as APIs and OS services). We will not go into too much detail about mocking right now, but the [Python documentation](https://docs.python.org/3/library/unittest.mock.html) contains great material on the subject. We recommend using these concepts liberally; in fact, in many cases you will not have much choice.

As you progress, one thing you may notice is that `unittest.mock.patch` doesn't play nice with relative module paths. This makes patching a method at the class or module level difficult.  One solution is to directly import the object using  `unittest.mock.patch.object`

```python
from unittest.mock import patch, ANY
from ..queue_block import Queue

@patch.object(Queue, '_load')
def test_it(self, load_patch):
	...
	load_patch.assert_called_once_with(ANY)
```
Again, you do not necessarily have to construct your tests in this manner; however, we have found this practice to be more convenient and less prone to user error than others.

---

## Mocking persistence module

To mock **load**, the persistence module:

```python
class TestPersistenceBlock(NIOBlockTestCase):

	def test_persist_load(self):
		blk = Block()
		with path('nio.modules.persistence.default.Persistence.load') as load:
			load.return_value = 'i was persisted'
			self.configure_block(blk, {})
```
To mock **save**:

```python
from unittest.mock import  MagicMock

class TestPersistenceBlock(NIOBlockTestCase):

	def test_persist_save(self):
		 blk = Block()
		 self.configure_block(blk, {})
		 blk.persistence.save = MagicMock()
```

---

## nio modules

`NIOBlockTestCase` configures the following nio modules by default: `['logging', 'scheduler', 'security', 'threading']`. If your block test case needs to use any other nio modules, you must specify by implementing the `get_test_modules` method.

If your test case uses persistence, enter the following code:

```python
class TestBlock(NIOBlockTestCase):

	def get_test_modules(self):
		return super().get_test_modules() + ['persistence']
```
You can override the default configuration of modules by implementing `get_module_config_*`. This ensures that the test case uses the `default` implementation of the persistence module.

```python
class TestBlock(NIOBlockTestCase):

	def get_module_config_persistence(self):
		return {'persistence': 'default'}
```
