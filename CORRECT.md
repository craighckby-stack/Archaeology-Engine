# Correct Commits Ledger (CORRECT.md)

> Full content and diffs of every commit that succeeded without failure or immediate reversion (newest commits first).

## Tue, 7 Jul 2026 15:30:28 -0700 -- PiperOrigin-RevId: 944126193 (`4a3bae0f`)

**Author:** Unknown

**Files touched:**
- `sonnet/__init__.py`
- `sonnet/src/base.py`
- `sonnet/src/batch_apply.py`
- `sonnet/src/bias.py`
- `sonnet/src/conformance/descriptors.py`
- `sonnet/src/conformance/goldens.py`
- `sonnet/src/conv.py`
- `sonnet/src/conv_transpose.py`
- `sonnet/src/custom_getter.py`
- `sonnet/src/embed.py`
- `sonnet/src/functional/haiku.py`
- `sonnet/src/functional/jax.py`
- `sonnet/src/initializers.py`
- `sonnet/src/metrics.py`
- `sonnet/src/nets/dnc/control.py`
- `sonnet/src/nets/resnet.py`
- `sonnet/src/optimizers/optimizer_tests.py`
- `sonnet/src/optimizers/optimizer_utils.py`
- `sonnet/src/recurrent.py`
- `sonnet/src/reshape.py`
- `sonnet/src/utils.py`

**Commit message:**
```
PiperOrigin-RevId: 944126193

```

**Diff:**
```diff
---
 sonnet/__init__.py                       |  2 +-
 sonnet/src/base.py                       | 14 +++++------
 sonnet/src/batch_apply.py                |  2 +-
 sonnet/src/bias.py                       | 10 ++++----
 sonnet/src/conformance/descriptors.py    |  2 +-
 sonnet/src/conformance/goldens.py        |  2 +-
 sonnet/src/conv.py                       |  2 +-
 sonnet/src/conv_transpose.py             | 10 ++++----
 sonnet/src/custom_getter.py              |  2 +-
 sonnet/src/embed.py                      |  2 +-
 sonnet/src/functional/haiku.py           |  6 ++---
 sonnet/src/functional/jax.py             |  2 +-
 sonnet/src/initializers.py               | 32 ++++++++++++------------
 sonnet/src/metrics.py                    |  2 +-
 sonnet/src/nets/dnc/control.py           |  4 +--
 sonnet/src/nets/resnet.py                |  8 +++---
 sonnet/src/optimizers/optimizer_tests.py |  2 +-
 sonnet/src/optimizers/optimizer_utils.py |  2 +-
 sonnet/src/recurrent.py                  | 24 +++++++++---------
 sonnet/src/reshape.py                    |  2 +-
 sonnet/src/utils.py                      | 10 ++++----
 21 files changed, 71 insertions(+), 71 deletions(-)

diff --git a/sonnet/__init__.py b/sonnet/__init__.py
index 629a1a2e..f2eecdbf 100644
--- a/sonnet/__init__.py
+++ b/sonnet/__init__.py
@@ -160,6 +160,6 @@
 #                 ||     ||
 #
 try:
-  del src  # pylint: disable=undefined-variable
+  del src  # pylint: disable=undefined-variable  # pyrefly: ignore[unbound-name]
 except NameError:
   pass
diff --git a/sonnet/src/base.py b/sonnet/src/base.py
index d845014f..3eb7b454 100644
--- a/sonnet/src/base.py
+++ b/sonnet/src/base.py
@@ -58,7 +58,7 @@ def no_name_scope(method: T) -> T:
 class ModuleMetaclass(abc.ABCMeta):
   """Metaclass for `Module`."""
 
-  def __new__(
+  def __new__(  # pyrefly: ignore[invalid-annotation]
       cls: Type[Type[T]],
       name: str,
       bases: Tuple[Type[Any], ...],
@@ -89,7 +89,7 @@ def __new__(
 
     clsdict.setdefault("__repr__", lambda module: module._auto_repr)  # pylint: disable=protected-access
 
-    new_cls = super(ModuleMetaclass, cls).__new__(cls, name, bases, clsdict)  # pylint: disable=too-many-function-args
+    new_cls = super(ModuleMetaclass, cls).__new__(cls, name, bases, clsdict)  # pylint: disable=too-many-function-args  # pyrefly: ignore[invalid-argument]
 
     for method_name in methods:
       # Note: the below is quite subtle, we need to ensure that we're wrapping
@@ -127,7 +127,7 @@ def __call__(cls: Type[T], *args, **kwargs) -> T:
       ctor_name_scope = getattr(module, "_ctor_name_scope", None)
       if ctor_name_scope is not None:
         ctor_name_scope.__exit__(*exc_info)
-        del module._ctor_name_scope
+        del module._ctor_name_scope  # pyrefly: ignore[missing-attribute]
 
       # TODO(tomhennigan) Remove `_scope_name` after next TF release.
       ran_super_ctor = (
@@ -139,7 +139,7 @@ def __call__(cls: Type[T], *args, **kwargs) -> T:
             "is not supported. Add the following as the first line in your "
             "__init__ method:\n\nsuper(%s, self).__init__()" % cls.__name__)
 
-    module._auto_repr = auto_repr(cls, *args, **kwargs)  # pylint: disable=protected-access
+    module._auto_repr = auto_repr(cls, *args, **kwargs)  # pylint: disable=protected-access  # pyrefly: ignore[missing-attribute]
 
     return module
 
@@ -357,12 +357,12 @@ def allow_empty_variables(module_or_cls: T) -> T:
 
 
 def assert_tf2():
-  if not assert_tf2.checked:
+  if not assert_tf2.checked:  # pyrefly: ignore[missing-attribute]
     with tf.init_scope():
       assert tf.executing_eagerly(), "Sonnet v2 requires TensorFlow 2"
-    assert_tf2.checked = True
+    assert_tf2.checked = True  # pyrefly: ignore[missing-attribute]
 
-assert_tf2.checked = False
+assert_tf2.checked = False  # pyrefly: ignore[missing-attribute]
 
 
 class Module(tf.Module, metaclass=ModuleMetaclass):
diff --git a/sonnet/src/batch_apply.py b/sonnet/src/batch_apply.py
index e6a6f0f4..f68f8954 100644
--- a/sonnet/src/batch_apply.py
+++ b/sonnet/src/batch_apply.py
@@ -127,7 +127,7 @@ def split_leading_dim(
 
 def maybe_prod(s: Sequence[Union[int, None]]) -> Optional[int]:
   try:
-    return np.prod(s)
+    return np.prod(s)  # pyrefly: ignore[no-matching-overload]
   except TypeError:
     # Can happen if the input contains `None`.
     return None
diff --git a/sonnet/src/bias.py b/sonnet/src/bias.py
index ff29c87d..5cfd2bde 100644
--- a/sonnet/src/bias.py
+++ b/sonnet/src/bias.py
@@ -92,13 +92,13 @@ def _initialize(self, inputs):
     utils.assert_minimum_rank(inputs, 2)
 
     input_shape = inputs.shape
-    bias_shape = calculate_bias_shape(input_shape, self.bias_dims)
+    bias_shape = calculate_bias_shape(input_shape, self.bias_dims)  # pyrefly: ignore[bad-argument-type]
 
     input_size = input_shape[1:]
     if self.output_size is not None:
       if self.output_size != input_size:
         raise ValueError("Input shape must be {} not {}".format(
-            (-1,) + self.output_size, input_shape))
+            (-1,) + self.output_size, input_shape))  # pyrefly: ignore[unsupported-operation]
 
     self.input_size = input_size
     self.b = tf.Variable(self.b_init(bias_shape, inputs.dtype), name="b")
@@ -141,10 +141,10 @@ def calculate_bias_shape(input_shape: types.ShapeLike,
     ValueError: If the user attempts to add bias over the mini-batch dimension,
         e.g. `bias_dims=[0]`.
   """
-  input_rank = len(input_shape)
+  input_rank = len(input_shape)  # pyrefly: ignore[bad-argument-type]
   if bias_dims is None:
     # If None, default is to use all dimensions.
-    return input_shape[1:]
+    return input_shape[1:]  # pyrefly: ignore[bad-index]
 
   elif not bias_dims:
     # If empty list, use a scalar bias.
@@ -165,7 +165,7 @@ def calculate_bias_shape(input_shape: types.ShapeLike,
             "Dimension %d (bias_dims=%r) out of range for input of rank %r." %
             (dim, tuple(bias_dims), input_rank))
 
-      bias_shape[dim] = input_shape[dim]
+      bias_shape[dim] = input_shape[dim]  # pyrefly: ignore[bad-index]
     # Strip leading unit dimensions.
     start = input_rank
     for dim in range(1, input_rank):
diff --git a/sonnet/src/conformance/descriptors.py b/sonnet/src/conformance/descriptors.py
index 985ff711..463af8e1 100644
--- a/sonnet/src/conformance/descriptors.py
+++ b/sonnet/src/conformance/descriptors.py
@@ -55,7 +55,7 @@ def __call__(self, x: tf.Tensor):
       return self.wrapped(x, initial_state)
     else:
       x = tf.expand_dims(x, axis=0)
-      return self.unroller(self.wrapped, x, initial_state)
+      return self.unroller(self.wrapped, x, initial_state)  # pyrefly: ignore[not-callable]
 
 
 def unwrap(module: snt.Module) -> snt.Module:
diff --git a/sonnet/src/conformance/goldens.py b/sonnet/src/conformance/goldens.py
index 41ce4a28..be0741c7 100644
--- a/sonnet/src/conformance/goldens.py
+++ b/sonnet/src/conformance/goldens.py
@@ -26,7 +26,7 @@
 
 
 def named_goldens() -> Sequence[Tuple[str, "Golden"]]:
-  return ((name, cls()) for _, name, cls in list_goldens())
+  return ((name, cls()) for _, name, cls in list_goldens())  # pyrefly: ignore[bad-return]
 
 
 def all_goldens(test_method):
diff --git a/sonnet/src/conv.py b/sonnet/src/conv.py
index 1e6576cc..a6113c9b 100644
--- a/sonnet/src/conv.py
+++ b/sonnet/src/conv.py
@@ -88,7 +88,7 @@ def __init__(self,
       self.padding_func = padding
 
     self.data_format = data_format
-    self._channel_index = utils.get_channel_index(data_format)
+    self._channel_index = utils.get_channel_index(data_format)  # pyrefly: ignore[bad-argument-type]
     self.with_bias = with_bias
 
     self.w_init = w_init
diff --git a/sonnet/src/conv_transpose.py b/sonnet/src/conv_transpose.py
index d2ba2fb1..21cd7eba 100644
--- a/sonnet/src/conv_transpose.py
+++ b/sonnet/src/conv_transpose.py
@@ -110,7 +110,7 @@ def __init__(self,
       raise TypeError("ConvNDTranspose only takes string padding, please "
                       "provide either `SAME` or `VALID`.")
     self._data_format = data_format
-    self._channel_index = utils.get_channel_index(data_format)
+    self._channel_index = utils.get_channel_index(data_format)  # pyrefly: ignore[bad-argument-type]
     self._with_bias = with_bias
 
     self._w_init = w_init
@@ -154,14 +154,14 @@ def _initialize(self, inputs):
     self._dtype = inputs.dtype
 
     if self._output_shape is not None:
-      if len(self._output_shape) != self._num_spatial_dims:
+      if len(self._output_shape) != self._num_spatial_dims:  # pyrefly: ignore[bad-argument-type]
         raise ValueError(
             "The output_shape must be of length {} but instead was {}.".format(
-                self._num_spatial_dims, len(self._output_shape)))
+                self._num_spatial_dims, len(self._output_shape)))  # pyrefly: ignore[bad-argument-type]
       if self._channel_index == 1:
-        self._output_shape = [self._output_channels] + list(self._output_shape)
+        self._output_shape = [self._output_channels] + list(self._output_shape)  # pyrefly: ignore[bad-argument-type]
       else:
-        self._output_shape = list(self._output_shape) + [self._output_channels]
+        self._output_shape = list(self._output_shape) + [self._output_channels]  # pyrefly: ignore[bad-argument-type]
 
     self.w = self._make_w()
     if self._with_bias:
diff --git a/sonnet/src/custom_getter.py b/sonnet/src/custom_getter.py
index e8e3def9..daf71ad4 100644
--- a/sonnet/src/custom_getter.py
+++ b/sonnet/src/custom_getter.py
@@ -87,7 +87,7 @@ def _custom_getter(
     orig_getattribute = cls.__getattribute__  # pytype: disable=attribute-error
 
     def new_getattribute(obj, name, orig_getattribute=orig_getattribute):
-      attr = orig_getattribute(obj, name)
+      attr = orig_getattribute(obj, name)  # pyrefly: ignore[bad-argument-count]
 
       if (instances is None) or (obj in instances):
         return getter(attr)
diff --git a/sonnet/src/embed.py b/sonnet/src/embed.py
index e87a5004..b9cb1e17 100644
--- a/sonnet/src/embed.py
+++ b/sonnet/src/embed.py
@@ -80,7 +80,7 @@ def __init__(self,
 
     if existing_vocab is None:
       if embed_dim is None:
-        embed_dim = embedding_dim(vocab_size)
+        embed_dim = embedding_dim(vocab_size)  # pyrefly: ignore[bad-argument-type]
       if initializer is None:
         initializer = initializers.TruncatedNormal()
       vocab = initializer([vocab_size, embed_dim], dtype)
diff --git a/sonnet/src/functional/haiku.py b/sonnet/src/functional/haiku.py
index b992ea29..45c886c9 100644
--- a/sonnet/src/functional/haiku.py
+++ b/sonnet/src/functional/haiku.py
@@ -64,7 +64,7 @@ def notify(f):
   """Wraps `f` such that callbacks are notified about it being called."""
   @functools.wraps(f)
   def wrapper(self, *args, **kwargs):
-    TensorVariableCallbacks.instance.notify(self)
+    TensorVariableCallbacks.instance.notify(self)  # pyrefly: ignore[missing-attribute]
     return f(self, *args, **kwargs)  # pytype: disable=wrong-arg-count
   return wrapper
 
@@ -252,7 +252,7 @@ def getter(next_getter, **kwargs):
 @contextlib.contextmanager
 def track_tensor_variables():
   tensor_variables = []
-  with TensorVariableCallbacks.instance(tensor_variables.append):  # pylint: disable=not-callable
+  with TensorVariableCallbacks.instance(tensor_variables.append):  # pylint: disable=not-callable  # pyrefly: ignore[not-callable]
     yield tensor_variables
 
 
@@ -276,7 +276,7 @@ def callback(v):
     if r not in var_state:
       var_state[r] = (v.initial_tensor_value, v.tensor_value)
 
-  with TensorVariableCallbacks.instance(callback):  # pylint: disable=not-callable
+  with TensorVariableCallbacks.instance(callback):  # pylint: disable=not-callable  # pyrefly: ignore[not-callable]
     yield var_state
 
 
diff --git a/sonnet/src/functional/jax.py b/sonnet/src/functional/jax.py
index 5ac8a0e3..cd318e11 100644
--- a/sonnet/src/functional/jax.py
+++ b/sonnet/src/functional/jax.py
@@ -64,7 +64,7 @@ def wrapper(*args, **kwargs):
       out, aux = out
     grads = tape.gradient(out, params)
     if has_aux:
-      return (out, aux), grads
+      return (out, aux), grads  # pyrefly: ignore[unbound-name]
     else:
       return out, grads
   return wrapper
diff --git a/sonnet/src/initializers.py b/sonnet/src/initializers.py
index a5144917..e4cf366f 100644
--- a/sonnet/src/initializers.py
+++ b/sonnet/src/initializers.py
@@ -182,12 +182,12 @@ def __call__(self, shape: types.ShapeLike, dtype: tf.DType) -> tf.Tensor:
       raise ValueError("The tensor to initialize must be "
                        "at least two-dimensional")
     elif rank == 2:
-      initializer = tf.eye(num_rows=shape[0], num_columns=shape[1], dtype=dtype)
+      initializer = tf.eye(num_rows=shape[0], num_columns=shape[1], dtype=dtype)  # pyrefly: ignore[bad-index]
     else:  # rank > 2
       initializer = tf.eye(
-          num_rows=shape[-2],
-          num_columns=shape[-1],
-          batch_shape=shape[:-2],
+          num_rows=shape[-2],  # pyrefly: ignore[bad-index]
+          num_columns=shape[-1],  # pyrefly: ignore[bad-index]
+          batch_shape=shape[:-2],  # pyrefly: ignore[bad-index]
           dtype=dtype)
     return self.gain * initializer
 
@@ -223,15 +223,15 @@ def __init__(self, gain: float = 1.0, seed: Optional[int] = None):
 
   def __call__(self, shape: types.ShapeLike, dtype: tf.DType) -> tf.Tensor:
     dtype = _as_floating_dtype(dtype)
-    if len(shape) < 2:
+    if len(shape) < 2:  # pyrefly: ignore[bad-argument-type]
       raise ValueError("The tensor to initialize must be "
                        "at least two-dimensional")
     # Flatten the input shape with the last dimension remaining
     # its original shape so it works for conv2d
     num_rows = 1
-    for dim in shape[:-1]:
+    for dim in shape[:-1]:  # pyrefly: ignore[bad-index]
       num_rows *= dim
-    num_cols = shape[-1]
+    num_cols = shape[-1]  # pyrefly: ignore[bad-index]
     flat_shape = [
         tf.maximum(num_cols, num_rows),
         tf.minimum(num_cols, num_rows)
@@ -370,21 +370,21 @@ def _compute_fans(shape: types.ShapeLike):
   Returns:
     A tuple of scalars `(fan_in, fan_out)`.
   """
-  if len(shape) < 1:  # Just to avoid errors for constants.
+  if len(shape) < 1:  # Just to avoid errors for constants.  # pyrefly: ignore[bad-argument-type]
     fan_in = fan_out = 1
-  elif len(shape) == 1:
-    fan_in = fan_out = shape[0]
-  elif len(shape) == 2:
-    fan_in = shape[0]
-    fan_out = shape[1]
+  elif len(shape) == 1:  # pyrefly: ignore[bad-argument-type]
+    fan_in = fan_out = shape[0]  # pyrefly: ignore[bad-index]
+  elif len(shape) == 2:  # pyrefly: ignore[bad-argument-type]
+    fan_in = shape[0]  # pyrefly: ignore[bad-index]
+    fan_out = shape[1]  # pyrefly: ignore[bad-index]
   else:
     # Assuming convolution kernels (2D, 3D, or more).
     # kernel shape: (..., input_depth, depth)
     receptive_field_size = 1.
-    for dim in shape[:-2]:
+    for dim in shape[:-2]:  # pyrefly: ignore[bad-index]
       receptive_field_size *= dim
-    fan_in = shape[-2] * receptive_field_size
-    fan_out = shape[-1] * receptive_field_size
+    fan_in = shape[-2] * receptive_field_size  # pyrefly: ignore[bad-index]
+    fan_out = shape[-1] * receptive_field_size  # pyrefly: ignore[bad-index]
   return fan_in, fan_out
 
 
diff --git a/sonnet/src/metrics.py b/sonnet/src/metrics.py
index 9e438350..327dc43c 100644
--- a/sonnet/src/metrics.py
+++ b/sonnet/src/metrics.py
@@ -22,7 +22,7 @@
 import tensorflow as tf
 
 
-class Metric(base.Module, metaclass=abc.ABCMeta):
+class Metric(base.Module, metaclass=abc.ABCMeta):  # pyrefly: ignore[invalid-inheritance]
   """Metric base class."""
 
   @abc.abstractmethod
diff --git a/sonnet/src/nets/dnc/control.py b/sonnet/src/nets/dnc/control.py
index 85f77059..2e8f0c25 100644
--- a/sonnet/src/nets/dnc/control.py
+++ b/sonnet/src/nets/dnc/control.py
@@ -83,7 +83,7 @@ def __call__(self, inputs, prev_state):
       output = self._activation(output)
     return output, prev_state
 
-  def initial_state(self, batch_size):
+  def initial_state(self, batch_size):  # pyrefly: ignore[bad-override]
     return tf.zeros([batch_size, 1], dtype=self.dtype)
 
 
@@ -110,6 +110,6 @@ def deep_core(control_name,
       for i in range(num_layers)
   ]
   if skip_connections:
-    return recurrent.deep_rnn_with_skip_connections(cores, name=name)
+    return recurrent.deep_rnn_with_skip_connections(cores, name=name)  # pyrefly: ignore[bad-argument-type]
   else:
     return recurrent.DeepRNN(cores, name=name)
diff --git a/sonnet/src/nets/resnet.py b/sonnet/src/nets/resnet.py
index d7254e42..08760068 100644
--- a/sonnet/src/nets/resnet.py
+++ b/sonnet/src/nets/resnet.py
@@ -41,7 +41,7 @@ def __init__(self,
     self._bn_config = bn_config
 
     batchnorm_args = {"create_scale": True, "create_offset": True}
-    batchnorm_args.update(bn_config)
+    batchnorm_args.update(bn_config)  # pyrefly: ignore[no-matching-overload]
 
     if self._use_projection:
       self._proj_conv = conv.Conv2D(
@@ -120,7 +120,7 @@ def __init__(self,
     self._bn_config = bn_config
 
     batchnorm_args = {"create_scale": True, "create_offset": True}
-    batchnorm_args.update(bn_config)
+    batchnorm_args.update(bn_config)  # pyrefly: ignore[no-matching-overload]
 
     if self._use_projection:
       self._proj_conv = conv.Conv2D(
@@ -274,7 +274,7 @@ def __init__(self,
           create_scale=True,
           create_offset=True,
           name="initial_batchnorm",
-          **bn_config)
+          **bn_config)  # pyrefly: ignore[bad-argument-type]
 
     self._block_groups = []
     strides = [1, 2, 2, 2]
@@ -293,7 +293,7 @@ def __init__(self,
           create_scale=True,
           create_offset=True,
           name="final_batchnorm",
-          **bn_config)
+          **bn_config)  # pyrefly: ignore[bad-argument-type]
 
     self._logits = linear.Linear(
         output_size=num_classes, w_init=initializers.Zeros(), name="logits")
diff --git a/sonnet/src/optimizers/optimizer_tests.py b/sonnet/src/optimizers/optimizer_tests.py
index ceb1e724..aea930e9 100644
--- a/sonnet/src/optimizers/optimizer_tests.py
+++ b/sonnet/src/optimizers/optimizer_tests.py
@@ -37,7 +37,7 @@ def __getattr__(self, name):
     return getattr(self.wrapped, name)
 
   def apply(self, updates, params):
-    self.wrapped.apply_gradients(zip(updates, params))
+    self.wrapped.apply_gradients(zip(updates, params))  # pyrefly: ignore[missing-attribute]
 
 
 def is_tf_optimizer(optimizer):
diff --git a/sonnet/src/optimizers/optimizer_utils.py b/sonnet/src/optimizers/optimizer_utils.py
index d904e2de..fc1df135 100644
--- a/sonnet/src/optimizers/optimizer_utils.py
+++ b/sonnet/src/optimizers/optimizer_utils.py
@@ -52,7 +52,7 @@ def check_updates_parameters(updates: Sequence[types.ParameterUpdate],
 
 
 def check_same_dtype(update: types.ParameterUpdate, parameter: tf.Variable):
-  if update.dtype != parameter.dtype:
+  if update.dtype != parameter.dtype:  # pyrefly: ignore[missing-attribute]
     raise ValueError(
         "DType of update {!r} is not equal to that of parameter {!r}".format(
             update, parameter))
diff --git a/sonnet/src/recurrent.py b/sonnet/src/recurrent.py
index 87a6e38a..96beb521 100644
--- a/sonnet/src/recurrent.py
+++ b/sonnet/src/recurrent.py
@@ -37,7 +37,7 @@
 # pylint: enable=g-direct-tensorflow-import
 
 
-class RNNCore(base.Module, metaclass=abc.ABCMeta):
+class RNNCore(base.Module, metaclass=abc.ABCMeta):  # pyrefly: ignore[invalid-inheritance]
   """Base class for Recurrent Neural Network cores.
 
   This class defines the basic functionality that every core should
@@ -81,7 +81,7 @@ def initial_state(self, batch_size: types.IntegerLike, **kwargs):
     """
 
 
-class UnrolledRNN(base.Module, metaclass=abc.ABCMeta):
+class UnrolledRNN(base.Module, metaclass=abc.ABCMeta):  # pyrefly: ignore[invalid-inheritance]
   """Base class for unrolled Recurrent Neural Networks.
 
   This class is a generalization of :class:`RNNCore` which operates on
@@ -528,7 +528,7 @@ def __call__(self, inputs: types.TensorNest,
     # For VanillaRNN, the next state of the RNN is the same as the outputs.
     return outputs, outputs
 
-  def initial_state(self, batch_size: int) -> tf.Tensor:
+  def initial_state(self, batch_size: int) -> tf.Tensor:  # pyrefly: ignore[bad-override]
     """See base class."""
     return tf.zeros(shape=[batch_size, self._hidden_size], dtype=self._dtype)
 
@@ -832,7 +832,7 @@ def __call__(self, inputs, prev_state):
     return _lstm_fn(inputs, prev_state, self._w_i, self._w_h, self.b,
                     self.projection)
 
-  def initial_state(self, batch_size: int) -> LSTMState:
+  def initial_state(self, batch_size: int) -> LSTMState:  # pyrefly: ignore[bad-override]
     """See base class."""
     return LSTMState(
         hidden=tf.zeros([batch_size, self._eff_hidden_size], dtype=self._dtype),
@@ -946,7 +946,7 @@ def __call__(self, input_sequence, initial_state):
     return _specialized_unrolled_lstm(input_sequence, initial_state, self._w_i,
                                       self._w_h, self.b)
 
-  def initial_state(self, batch_size):
+  def initial_state(self, batch_size):  # pyrefly: ignore[bad-override]
     """See base class."""
     return LSTMState(
         hidden=tf.zeros([batch_size, self._hidden_size], dtype=self._dtype),
@@ -1284,7 +1284,7 @@ def __init__(self,
     """
     super().__init__(name)
     self._num_spatial_dims = num_spatial_dims
-    self._input_shape = list(input_shape)
+    self._input_shape = list(input_shape)  # pyrefly: ignore[bad-argument-type]
     self._channel_index = 1 if (data_format is not None and
                                 data_format.startswith("NC")) else -1
     self._output_channels = output_channels
@@ -1336,7 +1336,7 @@ def input_to_hidden(self):
   def hidden_to_hidden(self):
     return self._hidden_to_hidden.w
 
-  def initial_state(self, batch_size):
+  def initial_state(self, batch_size):  # pyrefly: ignore[bad-override]
     """See base class."""
     shape = list(self._input_shape)
     shape[self._channel_index] = self._output_channels
@@ -1355,7 +1355,7 @@ def _initialize(self, inputs):
 
 
 class Conv1DLSTM(_ConvNDLSTM):  # pylint: disable=missing-docstring,empty-docstring
-  __doc__ = _ConvNDLSTM.__doc__.replace("``num_spatial_dims``", "1")
+  __doc__ = _ConvNDLSTM.__doc__.replace("``num_spatial_dims``", "1")  # pyrefly: ignore[missing-attribute]
 
   def __init__(self,
                input_shape: types.ShapeLike,
@@ -1406,7 +1406,7 @@ def __init__(self,
 
 
 class Conv2DLSTM(_ConvNDLSTM):  # pylint: disable=missing-docstring,empty-docstring
-  __doc__ = _ConvNDLSTM.__doc__.replace("``num_spatial_dims``", "2")
+  __doc__ = _ConvNDLSTM.__doc__.replace("``num_spatial_dims``", "2")  # pyrefly: ignore[missing-attribute]
 
   def __init__(self,
                input_shape: types.ShapeLike,
@@ -1457,7 +1457,7 @@ def __init__(self,
 
 
 class Conv3DLSTM(_ConvNDLSTM):  # pylint: disable=missing-docstring,empty-docstring
-  __doc__ = _ConvNDLSTM.__doc__.replace("``num_spatial_dims``", "3")
+  __doc__ = _ConvNDLSTM.__doc__.replace("``num_spatial_dims``", "3")  # pyrefly: ignore[missing-attribute]
 
   def __init__(self,
                input_shape: types.ShapeLike,
@@ -1584,7 +1584,7 @@ def __call__(self, inputs, prev_state):
     next_state = (1 - z) * prev_state + z * a
     return next_state, next_state
 
-  def initial_state(self, batch_size):
+  def initial_state(self, batch_size):  # pyrefly: ignore[bad-override]
     """See base class."""
     return tf.zeros([batch_size, self._hidden_size], dtype=self._dtype)
 
@@ -1710,7 +1710,7 @@ def input_to_hidden(self):
   def hidden_to_hidden(self):
     return self._w_h
 
-  def initial_state(self, batch_size):
+  def initial_state(self, batch_size):  # pyrefly: ignore[bad-override]
     """See base class."""
     return tf.zeros([batch_size, self._hidden_size], dtype=self._dtype)
 
diff --git a/sonnet/src/reshape.py b/sonnet/src/reshape.py
index d03abc73..bef9d8e6 100644
--- a/sonnet/src/reshape.py
+++ b/sonnet/src/reshape.py
@@ -143,7 +143,7 @@ def __call__(self, inputs: tf.Tensor) -> tf.Tensor:
     self._initialize(inputs)
 
     # Resolve the wildcard if any.
-    output_shape = tuple(self._output_shape)
+    output_shape = tuple(self._output_shape)  # pyrefly: ignore[bad-argument-type]
     if -1 in output_shape:
       reshaped_shape = inputs.shape[self._preserve_dims:]
       if reshaped_shape.is_fully_defined():
diff --git a/sonnet/src/utils.py b/sonnet/src/utils.py
index 07a9b7f8..eba18397 100644
--- a/sonnet/src/utils.py
+++ b/sonnet/src/utils.py
@@ -36,11 +36,11 @@ def replicate(
 ) -> Tuple[T]:
   """Replicates entry in `element` `num_times` if needed."""
   if not isinstance(element, collections.abc.Sequence):
-    return (element,) * num_times
+    return (element,) * num_times  # pyrefly: ignore[bad-return]
   elif len(element) == 1:
-    return tuple(element * num_times)
+    return tuple(element * num_times)  # pyrefly: ignore[bad-argument-type, unsupported-operation]
   elif len(element) == num_times:
-    return tuple(element)
+    return tuple(element)  # pyrefly: ignore[bad-return]
   raise TypeError(
       "{} must be a scalar or sequence of length 1 or sequence of length {}."
       .format(name, num_times))
@@ -70,7 +70,7 @@ def _decorate_object(*args, **kwargs):
 
       @functools.wraps(f)
       def _decorate_bound_method(*args, **kwargs):
-        return decorator_fn(f, f.__self__, args, kwargs)
+        return decorator_fn(f, f.__self__, args, kwargs)  # pyrefly: ignore[bad-argument-type]
 
       return _decorate_bound_method
 
@@ -92,7 +92,7 @@ def _decorate_fn(*args, **kwargs):
 
     return _decorate_fn
 
-  return _decorator
+  return _decorator  # pyrefly: ignore[bad-return]
 
 
 _SPATIAL_CHANNELS_FIRST = re.compile("^NC[^C]*$")



```

---

## Wed, 6 May 2026 13:18:07 -0700 -- PiperOrigin-RevId: 911511211 (`a3052a27`)

**Author:** Unknown

**Files touched:**
- `docs/ext/BUILD`
- `examples/BUILD`
- `sonnet/BUILD`
- `sonnet/nets/BUILD`
- `sonnet/src/BUILD`
- `sonnet/src/conformance/BUILD`
- `sonnet/src/conformance/checkpoints/BUILD`
- `sonnet/src/distribute/BUILD`
- `sonnet/src/functional/BUILD`
- `sonnet/src/nets/BUILD`
- `sonnet/src/nets/dnc/BUILD`
- `sonnet/src/optimizers/BUILD`

**Commit message:**
```
PiperOrigin-RevId: 911511211

```

**Diff:**
```diff
---
 docs/ext/BUILD                           | 2 ++
 examples/BUILD                           | 5 ++++-
 sonnet/BUILD                             | 8 +++++++-
 sonnet/nets/BUILD                        | 5 ++++-
 sonnet/src/BUILD                         | 5 ++++-
 sonnet/src/conformance/BUILD             | 1 +
 sonnet/src/conformance/checkpoints/BUILD | 5 ++++-
 sonnet/src/distribute/BUILD              | 5 ++++-
 sonnet/src/functional/BUILD              | 5 ++++-
 sonnet/src/nets/BUILD                    | 5 ++++-
 sonnet/src/nets/dnc/BUILD                | 2 ++
 sonnet/src/optimizers/BUILD              | 5 ++++-
 12 files changed, 44 insertions(+), 9 deletions(-)

diff --git a/docs/ext/BUILD b/docs/ext/BUILD
index 77358d20..4ba18921 100644
--- a/docs/ext/BUILD
+++ b/docs/ext/BUILD
@@ -2,6 +2,8 @@ load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
 licenses(["notice"])
 
+package(default_applicable_licenses = ["//sonnet:license"])  # copybara:strip
+
 snt_py_library(
     name = "link_tf_api",
     srcs = ["link_tf_api.py"],
diff --git a/examples/BUILD b/examples/BUILD
index 3b29b6e4..ec5fc929 100644
--- a/examples/BUILD
+++ b/examples/BUILD
@@ -2,7 +2,10 @@
 load("//third_party/bazel_rules/rules_python/python:py_binary.bzl", "py_binary")
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//visibility:private"])
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_visibility = ["//visibility:private"],
+)
 
 licenses(["notice"])
 
diff --git a/sonnet/BUILD b/sonnet/BUILD
index 7d072826..40d72079 100644
--- a/sonnet/BUILD
+++ b/sonnet/BUILD
@@ -1,6 +1,12 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library")
+load("//tools/build_defs/license:license.bzl", "license")
 
-package(default_visibility = ["//visibility:private"])
+package(
+    default_applicable_licenses = [":license"],  # copybara:strip
+    default_visibility = ["//visibility:private"],
+)
+
+license(name = "license")
 
 licenses(["notice"])
 
diff --git a/sonnet/nets/BUILD b/sonnet/nets/BUILD
index d6c31098..0c0fe4a6 100644
--- a/sonnet/nets/BUILD
+++ b/sonnet/nets/BUILD
@@ -1,6 +1,9 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library")
 
-package(default_visibility = ["//sonnet:__pkg__"])
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_visibility = ["//sonnet:__pkg__"],
+)
 
 licenses(["notice"])
 
diff --git a/sonnet/src/BUILD b/sonnet/src/BUILD
index d5266463..56eec8ec 100644
--- a/sonnet/src/BUILD
+++ b/sonnet/src/BUILD
@@ -1,6 +1,9 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"])
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],
+)
 
 licenses(["notice"])
 
diff --git a/sonnet/src/conformance/BUILD b/sonnet/src/conformance/BUILD
index ff8f65fa..56b3178f 100644
--- a/sonnet/src/conformance/BUILD
+++ b/sonnet/src/conformance/BUILD
@@ -1,6 +1,7 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
 package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
     default_testonly = True,
     default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],
 )
diff --git a/sonnet/src/conformance/checkpoints/BUILD b/sonnet/src/conformance/checkpoints/BUILD
index afb79a85..7c589eda 100644
--- a/sonnet/src/conformance/checkpoints/BUILD
+++ b/sonnet/src/conformance/checkpoints/BUILD
@@ -1,6 +1,9 @@
 load("//third_party/bazel_rules/rules_python/python:py_binary.bzl", "py_binary")
 
-package(default_testonly = True)
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_testonly = True,
+)
 
 licenses(["notice"])
 
diff --git a/sonnet/src/distribute/BUILD b/sonnet/src/distribute/BUILD
index 06e1f7ac..7b721bee 100644
--- a/sonnet/src/distribute/BUILD
+++ b/sonnet/src/distribute/BUILD
@@ -1,6 +1,9 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"])
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],
+)
 
 licenses(["notice"])
 
diff --git a/sonnet/src/functional/BUILD b/sonnet/src/functional/BUILD
index 45721a11..158df968 100644
--- a/sonnet/src/functional/BUILD
+++ b/sonnet/src/functional/BUILD
@@ -1,6 +1,9 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"])
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],
+)
 
 licenses(["notice"])
 
diff --git a/sonnet/src/nets/BUILD b/sonnet/src/nets/BUILD
index dc88e086..af329bd9 100644
--- a/sonnet/src/nets/BUILD
+++ b/sonnet/src/nets/BUILD
@@ -1,6 +1,9 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"])
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],
+)
 
 licenses(["notice"])
 
diff --git a/sonnet/src/nets/dnc/BUILD b/sonnet/src/nets/dnc/BUILD
index 1aee407e..ee8ff53e 100644
--- a/sonnet/src/nets/dnc/BUILD
+++ b/sonnet/src/nets/dnc/BUILD
@@ -5,6 +5,8 @@ load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
 licenses(["notice"])
 
+package(default_applicable_licenses = ["//sonnet:license"])  # copybara:strip
+
 snt_py_library(
     name = "control",
     srcs = ["control.py"],
diff --git a/sonnet/src/optimizers/BUILD b/sonnet/src/optimizers/BUILD
index 6ae8210f..f5d0731b 100644
--- a/sonnet/src/optimizers/BUILD
+++ b/sonnet/src/optimizers/BUILD
@@ -1,6 +1,9 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"])
+package(
+    default_applicable_licenses = ["//sonnet:license"],  # copybara:strip
+    default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],
+)
 
 licenses(["notice"])
 



```

---

## Tue, 10 Feb 2026 08:28:28 -0800 -- PiperOrigin-RevId: 868160058 (`e501ca17`)

**Author:** Unknown

**Files touched:**
- `examples/BUILD`
- `sonnet/src/conformance/checkpoints/BUILD`

**Commit message:**
```
PiperOrigin-RevId: 868160058

```

**Diff:**
```diff
---
 examples/BUILD                           | 2 ++
 sonnet/src/conformance/checkpoints/BUILD | 1 +
 2 files changed, 3 insertions(+)

diff --git a/examples/BUILD b/examples/BUILD
index a6847ed8..3b29b6e4 100644
--- a/examples/BUILD
+++ b/examples/BUILD
@@ -9,6 +9,7 @@ licenses(["notice"])
 py_binary(
     name = "simple_mnist",
     srcs = ["simple_mnist.py"],
+    strict_deps = False,
     deps = [
         # pip: absl:app
         "//sonnet",
@@ -42,6 +43,7 @@ snt_py_test(
 py_binary(
     name = "functional_mlp_mnist",
     srcs = ["functional_mlp_mnist.py"],
+    strict_deps = False,
     deps = [
         # pip: absl:app
         # pip: absl/logging
diff --git a/sonnet/src/conformance/checkpoints/BUILD b/sonnet/src/conformance/checkpoints/BUILD
index 30980073..afb79a85 100644
--- a/sonnet/src/conformance/checkpoints/BUILD
+++ b/sonnet/src/conformance/checkpoints/BUILD
@@ -7,6 +7,7 @@ licenses(["notice"])
 py_binary(
     name = "generate",
     srcs = ["generate.py"],
+    strict_deps = False,
     deps = [
         # pip: absl:app
         # pip: absl/flags



```

---

## Wed, 14 Jan 2026 06:42:27 -0800 -- PiperOrigin-RevId: 856190293 (`2f59e0f0`)

**Author:** Unknown

**Files touched:**
- `examples/distributed_cifar10.ipynb`
- `examples/little_gan_on_mnist.ipynb`
- `examples/mlp_on_mnist.ipynb`

**Commit message:**
```
PiperOrigin-RevId: 856190293

```

**Diff:**
```diff
---
 examples/distributed_cifar10.ipynb |  2 +-
 examples/little_gan_on_mnist.ipynb | 64 +++++++++++++++---------------
 examples/mlp_on_mnist.ipynb        | 22 +++++-----
 3 files changed, 44 insertions(+), 44 deletions(-)

diff --git a/examples/distributed_cifar10.ipynb b/examples/distributed_cifar10.ipynb
index dac68545..98e7024f 100644
--- a/examples/distributed_cifar10.ipynb
+++ b/examples/distributed_cifar10.ipynb
@@ -59,7 +59,7 @@
       "outputs": [],
       "source": [
         "import sys\n",
-        "assert sys.version_info \u003e= (3, 6), \"Sonnet 2 requires Python \u003e=3.6\""
+        "assert sys.version_info >= (3, 6), \"Sonnet 2 requires Python >=3.6\""
       ]
     },
     {
diff --git a/examples/little_gan_on_mnist.ipynb b/examples/little_gan_on_mnist.ipynb
index 98dcfedc..adfce5aa 100644
--- a/examples/little_gan_on_mnist.ipynb
+++ b/examples/little_gan_on_mnist.ipynb
@@ -68,7 +68,7 @@
       "outputs": [],
       "source": [
         "import sys\n",
-        "assert sys.version_info \u003e= (3, 6), \"Sonnet 2 requires Python \u003e=3.6\""
+        "assert sys.version_info >= (3, 6), \"Sonnet 2 requires Python >=3.6\""
       ]
     },
     {
@@ -236,7 +236,7 @@
         {
           "data": {
             "text/plain": [
-              "\u003cmatplotlib.image.AxesImage at 0x7fb4d006c438\u003e"
+              "<matplotlib.image.AxesImage at 0x7fb4d006c438>"
             ]
           },
           "execution_count": 8,
@@ -249,7 +249,7 @@
           "data": {
             "image/png": "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\n",
             "text/plain": [
-              "\u003cFigure size 432x288 with 1 Axes\u003e"
+              "<Figure size 432x288 with 1 Axes>"
             ]
           },
           "metadata": {
@@ -515,7 +515,7 @@
           "data": {
             "image/png": "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\nrkJggg==\n",
             "text/plain": [
-              "\u003cFigure size 1440x144 with 20 Axes\u003e"
+              "<Figure size 1440x144 with 20 Axes>"
             ]
           },
           "metadata": {
@@ -724,7 +724,7 @@
         "  def _get_lr_mult(self, epoch):\n",
         "    # Linear decay to 0.\n",
         "    decay_epoch = tf.cast(epoch - self.decay_lr_start_epoch, tf.float32)\n",
-        "    if decay_epoch \u003c tf.constant(0, dtype=tf.float32):\n",
+        "    if decay_epoch < tf.constant(0, dtype=tf.float32):\n",
         "      return tf.constant(1., dtype=tf.float32)\n",
         "    num_decay_epochs = tf.cast(self.num_epochs - self.decay_lr_start_epoch,\n",
         "                               dtype=tf.float32)\n",
@@ -769,7 +769,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            "\r  0%|          | 0/750000 [00:00\u003c?, ?images/s]"
+            "\r  0%|          | 0/750000 [00:00<?, ?images/s]"
           ]
         },
         {
@@ -788,7 +788,7 @@
             "WARNING:tensorflow:From /tensorflow-2.0.0-rc1/python3.6/tensorflow_core/python/ops/resource_variable_ops.py:1781: calling BaseResourceVariable.__init__ (from tensorflow.python.ops.resource_variable_ops) with constraint is deprecated and will be removed in a future version.\n",
             "Instructions for updating:\n",
             "If using Keras pass *_constraint arguments to layers.\n",
-            "  4%|▍         | 31300/750000 [00:19\u003c01:15, 9470.95images/s]"
+            "  4%|▍         | 31300/750000 [00:19<01:15, 9470.95images/s]"
           ]
         },
         {
@@ -803,7 +803,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            "  8%|▊         | 62500/750000 [00:21\u003c00:56, 12207.04images/s]"
+            "  8%|▊         | 62500/750000 [00:21<00:56, 12207.04images/s]"
           ]
         },
         {
@@ -818,7 +818,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 12%|█▏        | 92400/750000 [00:24\u003c00:53, 12197.78images/s]"
+            " 12%|█▏        | 92400/750000 [00:24<00:53, 12197.78images/s]"
           ]
         },
         {
@@ -833,7 +833,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 16%|█▋        | 122200/750000 [00:26\u003c00:51, 12084.79images/s]"
+            " 16%|█▋        | 122200/750000 [00:26<00:51, 12084.79images/s]"
           ]
         },
         {
@@ -848,7 +848,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 20%|██        | 151900/750000 [00:28\u003c00:49, 12153.37images/s]"
+            " 20%|██        | 151900/750000 [00:28<00:49, 12153.37images/s]"
           ]
         },
         {
@@ -863,7 +863,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 24%|██▍       | 181800/750000 [00:31\u003c00:47, 11858.25images/s]"
+            " 24%|██▍       | 181800/750000 [00:31<00:47, 11858.25images/s]"
           ]
         },
         {
@@ -878,7 +878,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 28%|██▊       | 211500/750000 [00:33\u003c00:44, 12015.89images/s]"
+            " 28%|██▊       | 211500/750000 [00:33<00:44, 12015.89images/s]"
           ]
         },
         {
@@ -893,7 +893,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 32%|███▏      | 242500/750000 [00:36\u003c00:41, 12089.66images/s]"
+            " 32%|███▏      | 242500/750000 [00:36<00:41, 12089.66images/s]"
           ]
         },
         {
@@ -908,7 +908,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 36%|███▋      | 272400/750000 [00:38\u003c00:39, 11972.72images/s]"
+            " 36%|███▋      | 272400/750000 [00:38<00:39, 11972.72images/s]"
           ]
         },
         {
@@ -923,7 +923,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 40%|████      | 302100/750000 [00:41\u003c00:38, 11631.19images/s]"
+            " 40%|████      | 302100/750000 [00:41<00:38, 11631.19images/s]"
           ]
         },
         {
@@ -938,7 +938,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 44%|████▍     | 331900/750000 [00:43\u003c00:35, 11762.71images/s]"
+            " 44%|████▍     | 331900/750000 [00:43<00:35, 11762.71images/s]"
           ]
         },
         {
@@ -953,7 +953,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 48%|████▊     | 361700/750000 [00:46\u003c00:33, 11701.33images/s]"
+            " 48%|████▊     | 361700/750000 [00:46<00:33, 11701.33images/s]"
           ]
         },
         {
@@ -968,7 +968,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 52%|█████▏    | 391300/750000 [00:48\u003c00:30, 11574.55images/s]"
+            " 52%|█████▏    | 391300/750000 [00:48<00:30, 11574.55images/s]"
           ]
         },
         {
@@ -983,7 +983,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 56%|█████▋    | 422000/750000 [00:51\u003c00:27, 11761.06images/s]"
+            " 56%|█████▋    | 422000/750000 [00:51<00:27, 11761.06images/s]"
           ]
         },
         {
@@ -998,7 +998,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 60%|██████    | 451400/750000 [00:53\u003c00:25, 11703.31images/s]"
+            " 60%|██████    | 451400/750000 [00:53<00:25, 11703.31images/s]"
           ]
         },
         {
@@ -1013,7 +1013,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 64%|██████▍   | 481900/750000 [00:56\u003c00:23, 11577.90images/s]"
+            " 64%|██████▍   | 481900/750000 [00:56<00:23, 11577.90images/s]"
           ]
         },
         {
@@ -1028,7 +1028,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 68%|██████▊   | 511200/750000 [00:58\u003c00:20, 11707.29images/s]"
+            " 68%|██████▊   | 511200/750000 [00:58<00:20, 11707.29images/s]"
           ]
         },
         {
@@ -1043,7 +1043,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 72%|███████▏  | 541700/750000 [01:01\u003c00:17, 12108.87images/s]"
+            " 72%|███████▏  | 541700/750000 [01:01<00:17, 12108.87images/s]"
           ]
         },
         {
@@ -1058,7 +1058,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 76%|███████▌  | 571500/750000 [01:03\u003c00:15, 11730.40images/s]"
+            " 76%|███████▌  | 571500/750000 [01:03<00:15, 11730.40images/s]"
           ]
         },
         {
@@ -1073,7 +1073,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 80%|████████  | 601400/750000 [01:06\u003c00:12, 12025.96images/s]"
+            " 80%|████████  | 601400/750000 [01:06<00:12, 12025.96images/s]"
           ]
         },
         {
@@ -1088,7 +1088,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 84%|████████▍ | 632400/750000 [01:08\u003c00:09, 11889.06images/s]"
+            " 84%|████████▍ | 632400/750000 [01:08<00:09, 11889.06images/s]"
           ]
         },
         {
@@ -1103,7 +1103,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 88%|████████▊ | 662100/750000 [01:11\u003c00:07, 11857.17images/s]"
+            " 88%|████████▊ | 662100/750000 [01:11<00:07, 11857.17images/s]"
           ]
         },
         {
@@ -1118,7 +1118,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 92%|█████████▏| 691900/750000 [01:13\u003c00:04, 12035.37images/s]"
+            " 92%|█████████▏| 691900/750000 [01:13<00:04, 12035.37images/s]"
           ]
         },
         {
@@ -1133,7 +1133,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            " 96%|█████████▌| 721800/750000 [01:16\u003c00:02, 11965.87images/s]"
+            " 96%|█████████▌| 721800/750000 [01:16<00:02, 11965.87images/s]"
           ]
         },
         {
@@ -1148,7 +1148,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            "100%|██████████| 750000/750000 [01:18\u003c00:00, 9546.01images/s] "
+            "100%|██████████| 750000/750000 [01:18<00:00, 9546.01images/s] "
           ]
         },
         {
@@ -1242,7 +1242,7 @@
           "data": {
             "image/png": "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\nAABJRU5ErkJggg==\n",
             "text/plain": [
-              "\u003cFigure size 720x72 with 10 Axes\u003e"
+              "<Figure size 720x72 with 10 Axes>"
             ]
           },
           "metadata": {
diff --git a/examples/mlp_on_mnist.ipynb b/examples/mlp_on_mnist.ipynb
index 4200eb0f..75b37b5e 100644
--- a/examples/mlp_on_mnist.ipynb
+++ b/examples/mlp_on_mnist.ipynb
@@ -45,7 +45,7 @@
       "outputs": [],
       "source": [
         "import sys\n",
-        "assert sys.version_info \u003e= (3, 6), \"Sonnet 2 requires Python \u003e=3.6\""
+        "assert sys.version_info >= (3, 6), \"Sonnet 2 requires Python >=3.6\""
       ]
     },
     {
@@ -68,10 +68,10 @@
             "Requirement already satisfied: dm-sonnet==2.0.0b0 in /usr/local/lib/python3.6/dist-packages (2.0.0b0)\n",
             "Requirement already satisfied: gast==0.2.2 in /usr/local/lib/python3.6/dist-packages (0.2.2)\n",
             "Requirement already satisfied: tqdm in /usr/local/lib/python3.6/dist-packages (4.28.1)\n",
-            "Requirement already satisfied: wrapt\u003e=1.11.1 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (1.11.2)\n",
-            "Requirement already satisfied: numpy\u003e=1.16.3 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (1.17.2)\n",
-            "Requirement already satisfied: absl-py\u003e=0.7.1 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (0.8.0)\n",
-            "Requirement already satisfied: six\u003e=1.12.0 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (1.12.0)\n"
+            "Requirement already satisfied: wrapt>=1.11.1 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (1.11.2)\n",
+            "Requirement already satisfied: numpy>=1.16.3 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (1.17.2)\n",
+            "Requirement already satisfied: absl-py>=0.7.1 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (0.8.0)\n",
+            "Requirement already satisfied: six>=1.12.0 in /tensorflow-2.0.0-rc1/python3.6 (from dm-sonnet==2.0.0b0) (1.12.0)\n"
           ]
         }
       ],
@@ -225,7 +225,7 @@
           "data": {
             "image/png": "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\n",
             "text/plain": [
-              "\u003cFigure size 432x288 with 1 Axes\u003e"
+              "<Figure size 432x288 with 1 Axes>"
             ]
           },
           "metadata": {
@@ -389,7 +389,7 @@
           "data": {
             "image/png": "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\n",
             "text/plain": [
-              "\u003cFigure size 432x288 with 1 Axes\u003e"
+              "<Figure size 432x288 with 1 Axes>"
             ]
           },
           "metadata": {
@@ -470,7 +470,7 @@
           "name": "stderr",
           "output_type": "stream",
           "text": [
-            "100%|██████████| 600000/600000 [01:02\u003c00:00, 9660.48images/s] "
+            "100%|██████████| 600000/600000 [01:02<00:00, 9660.48images/s] "
           ]
         },
         {
@@ -592,7 +592,7 @@
         "  n = 0\n",
         "\n",
         "  f, ax = plt.subplots(rows, cols)\n",
-        "  if rows \u003e 1:    \n",
+        "  if rows > 1:    \n",
         "    ax = tf.nest.flatten([tuple(ax[i]) for i in range(rows)])\n",
         "  f.set_figwidth(14)\n",
         "  f.set_figheight(4 * rows)\n",
@@ -635,7 +635,7 @@
           "data": {
             "image/png": "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\n17ZRHmnNoJn1MLOpZva2ma0ws2fN7ItdvV1URlpzmGzvZjNbbGYfmdmrZnZCObaL8kp5Bj9ucWsy\nsyvLsW2UV1pzWIufyQxk2mBmW0jaU7lrDxxS0c4UYGb7S/qRpK9L6itpL0lvVLRTKKmUZ7BeuSsU\n7y2pv6TzJd2e9BlVJOU5lHLXXtjC3fsp17+LzGyXCvcJJZT2DLp7nw03SUMlrRYXu6w6Kc9hzX0m\nM5Bp27GSZkqaptzFqyRJZtbLzC5LRr1/M7PHzayXpMeSJh8mf5HZzcwmm9nNzR6bNzo3s68nR1RW\nmNkbZnZyO/r3A0kXuvtMd1/v7ovcfVFnnzRSJbUZdPeV7j7Z3d9K8nevpDcl8Qtk9UltDiXJ3ee6\n+9oNi8ltq049Y6RNqjPYwlckLZP0pw4+HumV2hzW4mcyA5m2HSvpluT2T2Y2JKlfqlwwPqfc1Vi/\nI2m9ckdEJGnT5C8zTxaxjWWSviypn3JHVn5sZjsXamhm15jZNcnPdZImSNrMzOab2UIzuyr5j4Pq\nkdoMFrhviKRtJM0t5okhU1Kfw6S2StLLkhZLuq8dzw/pl/oMNjNJ0s+dq45Xo8zksBY+k8s2jyOL\nzGwPSWMk3e7u75nZ65L+1cx+IukbknZtdvTjieQx7d6Ou/+22eIfzewB5Q5bPlOg7TebLQ6R1CDp\nq0n7dZJmKHco8fvt7ghSJwMZbN7XBuV27NPd/eV2dwKplZUcuvs3zewMSbtJ+ryktS3bIJuyksFk\nu2OU+2rP8e3uAFItYzmsic9kjshs3CRJD7j7e8nyL5LaYEk9Jb1eio2Y2RfNbKaZfWBmH0o6KNlG\nW1Yn/17p7ouTfl6ePB7VIe0Z3PD4bpJukvSJpNNL0SekSiZyKEnu3uTuj0saKenUUvQLqZCZDEo6\nRtLj7v5mKfqEVMlEDmvpM5kjMq1Ivp71NUl1ZrYkKfeQtKmkYZLWKPf96+daPLTQYeSVkno3Wx7a\nbDs9JN2h3KHKGe6+zszultTmEN7dl1vuFHzNt8lh7CqRhQwmjzdJU5U7QniQu68r5nHIhqzksIB6\nMUemKmQwg8dKuqSdj0HKZSWHtfaZzBGZ1h0mqUnSeEk7JrftlJu4d6ykGyRdbmbDzawumbzVQ9K7\nyn0ncstm63pW0l5mNtrM+kv692b3dVfuP8K7khotd5q8A9rRzxslnWFmm5vZAEn/June9j9dpFBW\n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\n5MF1f//wc0EAAAAASUVORK5CYII=\n",
             "text/plain": [
-              "\u003cFigure size 1008x288 with 5 Axes\u003e"
+              "<Figure size 1008x288 with 5 Axes>"
             ]
           },
           "metadata": {
@@ -675,7 +675,7 @@
           "data": {
             "image/png": "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\ny70XQugWQqgvdZui/1T3mtlSM1tkZles+w+G8qnyDD5mZqui1/6pmb1Z6m2gMNWcw2h7h5jZTDP7\nzMzeNrPd2mI7aFq1ZtDMOprZdWY218xWmNk0Mxtbym2gcNWaw2hbnzZ61JvZ5aXeTluquo5M5Bsh\nhG6StpM0RtLZ2U9aRiW+NxMkLZbUV9JoSbtL+lGsLape1ZpBSTo52il3CyFsHndjqlxV5tDM9pH0\nW0nHSOou6SuS3om1UdWrGjPYTtI8ZX4Hb6DMa77VzIbE2KZqV405VNbv4m6SNpX0uaS/x9ysFqm4\nD6UlQggLJP1L0lbRN8UXmNlTklZKGmpmG0Tfmiw0swVmdv66Q3tmVmtmF5nZEjN7R9IB2euO1ndc\n1vQPo2//VpjZDDPbzsz+ImmQpH9GPeEz8xyK7GdmU8zsYzObbWY/zFrneDO71cz+HK13upmNWc9L\n3kzSrSGEVSGERZLuk/SFkryZKEoVZhAJVIU5/JWkc0MIz4YQGkIIC6L3ADGppgyGED4LIYwPIcyJ\n8ne3pHclbV/SNxUtVk05zOO7ynzZ/e/i38EYhBCq6iFpjqS9o58HSpou6TxJj0l6T5k/7NtJai/p\nDknXSOoqqbek5yUdHy17gqQ3onX0kvSopCCpXfT8Y5KOi34+SNICSTtIMknDJQ1u3J5oekij9Tyh\nzJGUTsocRflQ0p7Rc+MlrZK0v6RaSb+R9GzWuiZImpA1fbykP0vqIqm/pNclfTvuz6TaHlWewcei\n5ZdIekrSHnF/HtX6qNYcRs+vkXSWpNmS5ku6QlLnuD+TantUawbzvA99omW3iPszqcYHOfzPc49I\nGh/359Hizy/uBsQU2E8lLZM0N/pQO0cBOzdrvj6SVivrl5ukQyU9mvWBn5D13NfWE9j7JZ26nvbk\nDWz0n6FeUves538jaVL083hJD2U9N0rS5+t57VtKelHS2mgbkyRZ3J9JtT2qPINfUuZUno6SjpK0\nQtKwuD+TanxUaw4l9YvWO1WZ02w3VqZTfUHcn0m1Pao1g4222V7SQ5KuifvzqNYHOQySNDha72Zx\nfx4tfVTrQO8DQwgPZRfMTMqcs7rOYGV2MAuj56TMqXjr5unXaP6569neQElvF9HOfpI+DiGsaLSd\n7MOEi7J+Ximpk5m1CyGszV6RZc7tvE/SREm7SOom6XplzhM/s4i2oXWqLoOSFEJ4Lmtyspkdqsw3\n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\n",
             "text/plain": [
-              "\u003cFigure size 1008x576 with 10 Axes\u003e"
+              "<Figure size 1008x576 with 10 Axes>"
             ]
           },
           "metadata": {



```

---

## Mon, 4 Aug 2025 04:28:51 -0700 -- PiperOrigin-RevId: 790691673 (`39176f90`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/conformance/checkpoints/BUILD`

**Commit message:**
```
PiperOrigin-RevId: 790691673

```

**Diff:**
```diff
---
 sonnet/src/conformance/checkpoints/BUILD | 5 +----
 1 file changed, 1 insertion(+), 4 deletions(-)

diff --git a/sonnet/src/conformance/checkpoints/BUILD b/sonnet/src/conformance/checkpoints/BUILD
index d52dd8a4..30980073 100644
--- a/sonnet/src/conformance/checkpoints/BUILD
+++ b/sonnet/src/conformance/checkpoints/BUILD
@@ -1,9 +1,6 @@
 load("//third_party/bazel_rules/rules_python/python:py_binary.bzl", "py_binary")
 
-package(
-    default_testonly = True,
-    default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],
-)
+package(default_testonly = True)
 
 licenses(["notice"])
 



```

---

## Mon, 4 Aug 2025 02:35:25 -0700 -- PiperOrigin-RevId: 790660311 (`ae7b0508`)

**Author:** Unknown

**Files touched:**
- `docs/ext/BUILD`

**Commit message:**
```
PiperOrigin-RevId: 790660311

```

**Diff:**
```diff
---
 docs/ext/BUILD | 2 --
 1 file changed, 2 deletions(-)

diff --git a/docs/ext/BUILD b/docs/ext/BUILD
index 94db9938..77358d20 100644
--- a/docs/ext/BUILD
+++ b/docs/ext/BUILD
@@ -1,7 +1,5 @@
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"])
-
 licenses(["notice"])
 
 snt_py_library(



```

---

## Tue, 22 Jul 2025 22:42:15 -0700 -- PiperOrigin-RevId: 786140693 (`7080499e`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/nets/dnc/BUILD`

**Commit message:**
```
PiperOrigin-RevId: 786140693

```

**Diff:**
```diff
---
 sonnet/src/nets/dnc/BUILD | 2 --
 1 file changed, 2 deletions(-)

diff --git a/sonnet/src/nets/dnc/BUILD b/sonnet/src/nets/dnc/BUILD
index d1a3d761..1aee407e 100644
--- a/sonnet/src/nets/dnc/BUILD
+++ b/sonnet/src/nets/dnc/BUILD
@@ -3,8 +3,6 @@
 
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
-package(default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"])
-
 licenses(["notice"])
 
 snt_py_library(



```

---

## Mon, 9 Jun 2025 03:02:54 -0700 -- PiperOrigin-RevId: 769061984 (`9b0dbf4e`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/BUILD`
- `sonnet/src/custom_getter.py`
- `sonnet/src/distribute/BUILD`
- `sonnet/src/distribute/replicator.py`
- `sonnet/src/functional/BUILD`
- `sonnet/src/functional/haiku.py`
- `sonnet/src/mixed_precision.py`

**Commit message:**
```
PiperOrigin-RevId: 769061984

```

**Diff:**
```diff
---
 sonnet/src/BUILD                    | 2 --
 sonnet/src/custom_getter.py         | 2 +-
 sonnet/src/distribute/BUILD         | 1 -
 sonnet/src/distribute/replicator.py | 2 +-
 sonnet/src/functional/BUILD         | 1 -
 sonnet/src/functional/haiku.py      | 2 +-
 sonnet/src/mixed_precision.py       | 2 +-
 7 files changed, 4 insertions(+), 8 deletions(-)

diff --git a/sonnet/src/BUILD b/sonnet/src/BUILD
index 05bfd897..d5266463 100644
--- a/sonnet/src/BUILD
+++ b/sonnet/src/BUILD
@@ -195,7 +195,6 @@ snt_py_library(
     srcs = ["custom_getter.py"],
     deps = [
         ":base",
-        # pip: contextlib2
         # pip: tensorflow
         # pip: tree
     ],
@@ -637,7 +636,6 @@ snt_py_library(
     deps = [
         ":custom_getter",
         ":utils",
-        # pip: contextlib2
         # pip: tensorflow
         # pip: tree
     ],
diff --git a/sonnet/src/custom_getter.py b/sonnet/src/custom_getter.py
index e978f2da..e8e3def9 100644
--- a/sonnet/src/custom_getter.py
+++ b/sonnet/src/custom_getter.py
@@ -14,9 +14,9 @@
 # ============================================================================
 """Custom getter for module members."""
 
+import contextlib
 from typing import Any, Callable, ContextManager, Iterable, Optional, Type
 
-import contextlib
 from sonnet.src import base
 import tensorflow as tf
 import tree
diff --git a/sonnet/src/distribute/BUILD b/sonnet/src/distribute/BUILD
index 48d1ba64..06e1f7ac 100644
--- a/sonnet/src/distribute/BUILD
+++ b/sonnet/src/distribute/BUILD
@@ -35,7 +35,6 @@ snt_py_library(
     srcs = ["replicator.py"],
     deps = [
         # pip: absl/logging
-        # pip: contextlib2
         "//sonnet/src:initializers",
         # pip: tensorflow
     ],
diff --git a/sonnet/src/distribute/replicator.py b/sonnet/src/distribute/replicator.py
index 2febc5f6..01930d81 100644
--- a/sonnet/src/distribute/replicator.py
+++ b/sonnet/src/distribute/replicator.py
@@ -14,10 +14,10 @@
 # ============================================================================
 """Replicator Distribution Strategy."""
 
+import contextlib
 from typing import Callable, TypeVar
 
 from absl import logging
-import contextlib
 from sonnet.src import initializers
 import tensorflow as tf
 
diff --git a/sonnet/src/functional/BUILD b/sonnet/src/functional/BUILD
index 70211270..45721a11 100644
--- a/sonnet/src/functional/BUILD
+++ b/sonnet/src/functional/BUILD
@@ -9,7 +9,6 @@ snt_py_library(
     srcs = ["haiku.py"],
     deps = [
         ":utils",
-        # pip: contextlib2
         # pip: tensorflow
     ],
 )
diff --git a/sonnet/src/functional/haiku.py b/sonnet/src/functional/haiku.py
index 101ed596..b992ea29 100644
--- a/sonnet/src/functional/haiku.py
+++ b/sonnet/src/functional/haiku.py
@@ -15,11 +15,11 @@
 """Implements part of the Haiku ("Sonnet for JAX") API in TensorFlow 2."""
 
 import collections
+import contextlib
 import functools
 import itertools
 import threading
 
-import contextlib
 from sonnet.src.functional import utils
 import tensorflow as tf
 
diff --git a/sonnet/src/mixed_precision.py b/sonnet/src/mixed_precision.py
index 624fd0e0..98e9dbbd 100644
--- a/sonnet/src/mixed_precision.py
+++ b/sonnet/src/mixed_precision.py
@@ -14,8 +14,8 @@
 # ============================================================================
 """Mixed Precision Decorator for Sonnet 2."""
 
-import uuid
 import contextlib
+import uuid
 
 from sonnet.src import custom_getter
 from sonnet.src import utils



```

---

## Fri, 14 Feb 2025 03:41:14 -0800 -- PiperOrigin-RevId: 726859299 (`c99b4913`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/nets/dnc/control.py`

**Commit message:**
```
PiperOrigin-RevId: 726859299

```

**Diff:**
```diff
---
 sonnet/src/nets/dnc/control.py | 3 ---
 1 file changed, 3 deletions(-)

diff --git a/sonnet/src/nets/dnc/control.py b/sonnet/src/nets/dnc/control.py
index b9d74bcb..85f77059 100644
--- a/sonnet/src/nets/dnc/control.py
+++ b/sonnet/src/nets/dnc/control.py
@@ -99,9 +99,6 @@ def deep_core(control_name,
     control_config: Dictionary containing the configuration for the modules.
     num_layers: Number of layers.
     skip_connections: Boolean that indicates whether to use skip connections.
-      See documenation for sonnet.DeepRnn in
-      //learning/deepmind/tensorflow/sonnet/python/modules/basic_rnn.py for more
-      information.
     name: module name.
 
   Returns:



```

---

## Fri, 14 Feb 2025 02:43:53 -0800 -- PiperOrigin-RevId: 726844590 (`e61c5d5e`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/ci.yml`
- `setup.py`

**Commit message:**
```
PiperOrigin-RevId: 726844590

```

**Diff:**
```diff
---
 .github/workflows/ci.yml | 2 +-
 setup.py                 | 2 +-
 2 files changed, 2 insertions(+), 2 deletions(-)

diff --git a/.github/workflows/ci.yml b/.github/workflows/ci.yml
index d083a87c..630e704b 100644
--- a/.github/workflows/ci.yml
+++ b/.github/workflows/ci.yml
@@ -14,7 +14,7 @@ jobs:
     runs-on: "${{ matrix.os }}"
     strategy:
       matrix:
-        python-version: [3.8, 3.9, '3.10']
+        python-version: [3.9, '3.10', '3.11']
         os: [ubuntu-latest]
     steps:
     - uses: actions/checkout@v2
diff --git a/setup.py b/setup.py
index 17bb00ec..e1e3ddd1 100644
--- a/setup.py
+++ b/setup.py
@@ -53,9 +53,9 @@ def _parse_requirements(requirements_txt_path):
         'Intended Audience :: Science/Research',
         'License :: OSI Approved :: Apache Software License',
         'Programming Language :: Python :: 3',
-        'Programming Language :: Python :: 3.8',
         'Programming Language :: Python :: 3.9',
         'Programming Language :: Python :: 3.10',
+        'Programming Language :: Python :: 3.11',
         'Topic :: Scientific/Engineering :: Mathematics',
         'Topic :: Software Development :: Libraries :: Python Modules',
         'Topic :: Software Development :: Libraries',



```

---

## Thu, 30 Jan 2025 05:58:47 -0800 -- PiperOrigin-RevId: 721359767 (`f93a7212`)

**Author:** Unknown

**Files touched:**
- `examples/BUILD`
- `sonnet/src/BUILD`
- `sonnet/src/conformance/checkpoints/BUILD`

**Commit message:**
```
PiperOrigin-RevId: 721359767

```

**Diff:**
```diff
---
 examples/BUILD                           | 4 ----
 sonnet/src/BUILD                         | 1 -
 sonnet/src/conformance/checkpoints/BUILD | 2 --
 3 files changed, 7 deletions(-)

diff --git a/examples/BUILD b/examples/BUILD
index 6104ccdd..a6847ed8 100644
--- a/examples/BUILD
+++ b/examples/BUILD
@@ -9,8 +9,6 @@ licenses(["notice"])
 py_binary(
     name = "simple_mnist",
     srcs = ["simple_mnist.py"],
-    python_version = "PY3",
-    srcs_version = "PY3",
     deps = [
         # pip: absl:app
         "//sonnet",
@@ -44,8 +42,6 @@ snt_py_test(
 py_binary(
     name = "functional_mlp_mnist",
     srcs = ["functional_mlp_mnist.py"],
-    python_version = "PY3",
-    srcs_version = "PY3",
     deps = [
         # pip: absl:app
         # pip: absl/logging
diff --git a/sonnet/src/BUILD b/sonnet/src/BUILD
index 8d68f628..05bfd897 100644
--- a/sonnet/src/BUILD
+++ b/sonnet/src/BUILD
@@ -682,7 +682,6 @@ snt_py_test(
 snt_py_library(
     name = "types",
     srcs = ["types.py"],
-    srcs_version = "PY3",
     deps = [
         # pip: numpy
         # pip: tensorflow
diff --git a/sonnet/src/conformance/checkpoints/BUILD b/sonnet/src/conformance/checkpoints/BUILD
index 3d97db88..d52dd8a4 100644
--- a/sonnet/src/conformance/checkpoints/BUILD
+++ b/sonnet/src/conformance/checkpoints/BUILD
@@ -10,8 +10,6 @@ licenses(["notice"])
 py_binary(
     name = "generate",
     srcs = ["generate.py"],
-    python_version = "PY3",
-    srcs_version = "PY3",
     deps = [
         # pip: absl:app
         # pip: absl/flags



```

---

## Thu, 14 Nov 2024 02:24:19 -0800 -- PiperOrigin-RevId: 696445043 (`86260867`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/functional/haiku.py`

**Commit message:**
```
PiperOrigin-RevId: 696445043

```

**Diff:**
```diff
---
 sonnet/src/functional/haiku.py | 6 ++++--
 1 file changed, 4 insertions(+), 2 deletions(-)

diff --git a/sonnet/src/functional/haiku.py b/sonnet/src/functional/haiku.py
index 217670da..101ed596 100644
--- a/sonnet/src/functional/haiku.py
+++ b/sonnet/src/functional/haiku.py
@@ -78,6 +78,7 @@ def safe_read_tensor_value(variable):
 
   value = variable.tensor_value
   if value is None:
+    # pylint: disable=implicit-str-concat
     raise ValueError("".join((
         "Attempted to read a TensorVariable in a context where it has no ",
         "value. This commonly happens for one of two reasons:",
@@ -96,6 +97,7 @@ def safe_read_tensor_value(variable):
         "For (2) to read variable values inspect the result of a transformed",
         "function (e.g. look at the `params` dictionary returned from ",
         "`f.init(..)`).")))
+    # pylint: enable=implicit-str-concat
 
   return value
 
@@ -372,11 +374,11 @@ def transform_with_state(f) -> TransformedWithState:
   non-trainable state:
 
   >>> y, state = f.apply(params, state, 3.0)
-  >>> y.numpy()
+  >>> float(y.numpy())
   3.0
 
   >>> y, state = f.apply(params, state, 6.0)
-  >>> y.numpy()
+  >>> float(y.numpy())
   5.0
 
   Args:



```

---

## Mon, 28 Oct 2024 11:04:53 -0700 -- PiperOrigin-RevId: 690678726 (`31d3fdcb`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/metrics.py`

**Commit message:**
```
PiperOrigin-RevId: 690678726

```

**Diff:**
```diff
---
 sonnet/src/metrics.py | 24 ++++++++++++++++++++----
 1 file changed, 20 insertions(+), 4 deletions(-)

diff --git a/sonnet/src/metrics.py b/sonnet/src/metrics.py
index fb871a0c..9e438350 100644
--- a/sonnet/src/metrics.py
+++ b/sonnet/src/metrics.py
@@ -62,7 +62,13 @@ def initialize(self, value: tf.Tensor):
   def update(self, value: tf.Tensor):
     """See base class."""
     self.initialize(value)
-    self.sum.assign_add(value)
+    self._checked_sum.assign_add(value)
+
+  @property
+  def _checked_sum(self):
+    if self.sum is None:
+      raise ValueError("Metric is not initialized.  Call `initialize` first.")
+    return self.sum
 
   @property
   def value(self) -> tf.Tensor:
@@ -71,6 +77,8 @@ def value(self) -> tf.Tensor:
 
   def reset(self):
     """See base class."""
+    if self.sum is None:
+      raise ValueError("Metric is not initialized.  Call `initialize` first.")
     self.sum.assign(tf.zeros_like(self.sum))
 
 
@@ -90,15 +98,23 @@ def initialize(self, value: tf.Tensor):
   def update(self, value: tf.Tensor):
     """See base class."""
     self.initialize(value)
-    self.sum.assign_add(value)
+    self._checked_sum.assign_add(value)
     self.count.assign_add(1)
 
+  @property
+  def _checked_sum(self) -> tf.Variable:
+    if self.sum is None:
+      raise ValueError("Metric is not initialized.  Call `initialize` first.")
+    return self.sum
+
   @property
   def value(self) -> tf.Tensor:
     """See base class."""
     # TODO(cjfj): Assert summed type is floating-point?
-    return self.sum / tf.cast(self.count, dtype=self.sum.dtype)
+    return self._checked_sum / tf.cast(
+        self.count, dtype=self._checked_sum.dtype
+    )
 
   def reset(self):
-    self.sum.assign(tf.zeros_like(self.sum))
+    self._checked_sum.assign(tf.zeros_like(self._checked_sum))
     self.count.assign(0)



```

---

## Mon, 8 Apr 2024 13:20:19 -0700 -- PiperOrigin-RevId: 622933698 (`6d597251`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/moving_averages.py`

**Commit message:**
```
PiperOrigin-RevId: 622933698

```

**Diff:**
```diff
---
 sonnet/src/moving_averages.py | 12 +++++++-----
 1 file changed, 7 insertions(+), 5 deletions(-)

diff --git a/sonnet/src/moving_averages.py b/sonnet/src/moving_averages.py
index 4605e680..c0b71584 100644
--- a/sonnet/src/moving_averages.py
+++ b/sonnet/src/moving_averages.py
@@ -14,7 +14,7 @@
 # ============================================================================
 """Exponential moving average for Sonnet."""
 
-from typing import Optional
+from typing import Optional, cast
 
 from sonnet.src import metrics
 from sonnet.src import once
@@ -61,8 +61,8 @@ def __init__(self, decay: types.FloatLike, name: Optional[str] = None):
     self._counter = tf.Variable(
         0, trainable=False, dtype=tf.int64, name="counter")
 
-    self._hidden = None
-    self.average = None
+    self._hidden: tf.Variable = cast(tf.Variable, None)
+    self.average: tf.Variable = cast(tf.Variable, None)
 
   def update(self, value: tf.Tensor):
     """Applies EMA to the value given."""
@@ -82,8 +82,10 @@ def value(self) -> tf.Tensor:
   def reset(self):
     """Resets the EMA."""
     self._counter.assign(tf.zeros_like(self._counter))
-    self._hidden.assign(tf.zeros_like(self._hidden))
-    self.average.assign(tf.zeros_like(self.average))
+    if self._hidden is not None:
+      self._hidden.assign(tf.zeros_like(self._hidden))
+    if self.average is not None:
+      self.average.assign(tf.zeros_like(self.average))
 
   @once.once
   def initialize(self, value: tf.Tensor):



```

---

## Tue, 2 Apr 2024 09:38:14 -0700 -- CudnnRNN and CudnnRNNV2 are not compatible with cuDNN 9+, so (`6cc140ec`)

**Author:** Unknown

**Files touched:**
- `examples/BUILD`
- `sonnet/src/recurrent.py`

**Commit message:**
```
CudnnRNN and CudnnRNNV2 are not compatible with cuDNN 9+, so

```

**Diff:**
```diff
this change makes Sonnet use CudnnRNNV3 instead.

Note that this raises the minimum supported cuDNN version to 8.1
(which is below 8.9 - the minimum supported cuDNN version in Tensorflow anyway).

PiperOrigin-RevId: 621206198
---
 examples/BUILD          |  1 +
 sonnet/src/recurrent.py | 23 ++++++++++++++++++++---
 2 files changed, 21 insertions(+), 3 deletions(-)

diff --git a/examples/BUILD b/examples/BUILD
index f55d2f14..6104ccdd 100644
--- a/examples/BUILD
+++ b/examples/BUILD
@@ -1,3 +1,4 @@
+# buildifier: disable=out-of-order-load - Breaks copybara otherwise
 load("//third_party/bazel_rules/rules_python/python:py_binary.bzl", "py_binary")
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
diff --git a/sonnet/src/recurrent.py b/sonnet/src/recurrent.py
index 97b75b77..87a6e38a 100644
--- a/sonnet/src/recurrent.py
+++ b/sonnet/src/recurrent.py
@@ -1069,12 +1069,20 @@ def _block_unrolled_lstm(input_sequence, initial_state, w_i, w_h, b):
 
 def _cudnn_unrolled_lstm(input_sequence, initial_state, w_i, w_h, b):
   """GPU/CuDNN-RNN specialization of :class:`UnrolledLSTM`."""
+  max_sequence_length = tf.shape(input_sequence)[0]
+  batch_dim = tf.expand_dims(tf.shape(input_sequence)[1], axis=0)
+
+  # cuDNN 9+ always requires the sequence_length array argument to be present,
+  # so we generate it here with the max_sequence_length in all positions.
+  sequence_lengths = tf.broadcast_to(max_sequence_length, batch_dim)
+
   # Intuitively, concat/transpose is not free but we did not see
   # it significantly affecting performance in benchmarks.
-  output_sequence, all_hidden, all_cell, _ = tf.raw_ops.CudnnRNN(
+  output_sequence, all_hidden, all_cell, _, _ = tf.raw_ops.CudnnRNNV3(
       input=input_sequence,
       input_h=tf.expand_dims(initial_state.hidden, axis=0),
       input_c=tf.expand_dims(initial_state.cell, axis=0),
+      sequence_lengths=sequence_lengths,
       params=tf.concat(
           [
               tf.reshape(tf.transpose(w_i), [-1]),
@@ -1659,7 +1667,15 @@ def __call__(self, inputs, prev_state):
     w_hz, w_hr, w_ha = tf.split(self._w_h, num_or_size_splits=3, axis=1)
     b_z, b_r, b_a = tf.split(self.b, num_or_size_splits=3)
     b_h_zero = tf.zeros([self._hidden_size])
-    outputs, next_hidden, _, _ = tf.raw_ops.CudnnRNN(
+
+    max_sequence_length = tf.shape(inputs)[0]
+    batch_dim = tf.expand_dims(tf.shape(inputs)[1], axis=0)
+
+    # cuDNN 9+ always requires the sequence_length array argument to be present,
+    # so we generate it here with the max_sequence_length in all positions.
+    sequence_lengths = tf.broadcast_to(max_sequence_length, batch_dim)
+
+    outputs, next_hidden, _, _, _ = tf.raw_ops.CudnnRNNV3(
         input=inputs,
         input_h=tf.expand_dims(prev_state, axis=0),
         input_c=0,
@@ -1681,7 +1697,8 @@ def __call__(self, inputs, prev_state):
                 b_h_zero,
             ],
             axis=0),
-        rnn_mode="gru")
+        rnn_mode="gru",
+        sequence_lengths=sequence_lengths)
 
     return outputs, next_hidden
 



```

---

## Tue, 2 Jan 2024 03:17:13 -0800 -- PiperOrigin-RevId: 595072254 (`26b0518d`)

**Author:** Unknown

**Files touched:**
- `sonnet/__init__.py`

**Commit message:**
```
PiperOrigin-RevId: 595072254

```

**Diff:**
```diff
---
 sonnet/__init__.py | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/sonnet/__init__.py b/sonnet/__init__.py
index 2b76d05b..629a1a2e 100644
--- a/sonnet/__init__.py
+++ b/sonnet/__init__.py
@@ -147,7 +147,7 @@
     "static_unroll",
 )
 
-__version__ = "2.0.2"
+__version__ = "2.0.3.dev"
 
 #  ________________________________________
 # / Please don't use symbols in `src` they \



```

---

## Tue, 2 Jan 2024 02:54:53 -0800 -- PiperOrigin-RevId: 595068296 (`95d81587`)

**Author:** Unknown

**Files touched:**
- `sonnet/__init__.py`

**Commit message:**
```
PiperOrigin-RevId: 595068296

```

**Diff:**
```diff
---
 sonnet/__init__.py | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/sonnet/__init__.py b/sonnet/__init__.py
index 714e8448..2b76d05b 100644
--- a/sonnet/__init__.py
+++ b/sonnet/__init__.py
@@ -147,7 +147,7 @@
     "static_unroll",
 )
 
-__version__ = "2.0.2.dev"
+__version__ = "2.0.2"
 
 #  ________________________________________
 # / Please don't use symbols in `src` they \



```

---

## Tue, 2 Jan 2024 02:27:52 -0800 -- PiperOrigin-RevId: 595063851 (`03ab1852`)

**Author:** Unknown

**Files touched:**
- `sonnet/__init__.py`

**Commit message:**
```
PiperOrigin-RevId: 595063851

```

**Diff:**
```diff
---
 sonnet/__init__.py | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

diff --git a/sonnet/__init__.py b/sonnet/__init__.py
index b2fbf495..714e8448 100644
--- a/sonnet/__init__.py
+++ b/sonnet/__init__.py
@@ -71,10 +71,10 @@
 from sonnet.src.recurrent import UnrolledLSTM
 from sonnet.src.recurrent import UnrolledRNN
 from sonnet.src.recurrent import VanillaRNN
-from sonnet.src.reshape import flatten
 from sonnet.src.reshape import Flatten
-from sonnet.src.reshape import reshape
+from sonnet.src.reshape import flatten
 from sonnet.src.reshape import Reshape
+from sonnet.src.reshape import reshape
 from sonnet.src.scale_gradient import scale_gradient
 from sonnet.src.sequential import Sequential
 from sonnet.src.utils import format_variables



```

---

## Fri, 21 Jul 2023 02:08:43 -0700 -- PiperOrigin-RevId: 549884461 (`12a7c790`)

**Author:** Unknown

**Files touched:**
- `examples/BUILD`
- `sonnet/src/conformance/checkpoints/BUILD`

**Commit message:**
```
PiperOrigin-RevId: 549884461

```

**Diff:**
```diff
---
 examples/BUILD                           | 1 +
 sonnet/src/conformance/checkpoints/BUILD | 2 ++
 2 files changed, 3 insertions(+)

diff --git a/examples/BUILD b/examples/BUILD
index 86687f74..f55d2f14 100644
--- a/examples/BUILD
+++ b/examples/BUILD
@@ -1,3 +1,4 @@
+load("//third_party/bazel_rules/rules_python/python:py_binary.bzl", "py_binary")
 load("//sonnet/src:build_defs.bzl", "snt_py_library", "snt_py_test")
 
 package(default_visibility = ["//visibility:private"])
diff --git a/sonnet/src/conformance/checkpoints/BUILD b/sonnet/src/conformance/checkpoints/BUILD
index 9905cca2..3d97db88 100644
--- a/sonnet/src/conformance/checkpoints/BUILD
+++ b/sonnet/src/conformance/checkpoints/BUILD
@@ -1,3 +1,5 @@
+load("//third_party/bazel_rules/rules_python/python:py_binary.bzl", "py_binary")
+
 package(
     default_testonly = True,
     default_visibility = ["//sonnet:__subpackages__", "//docs/ext:__subpackages__", "//examples:__subpackages__"],



```

---

## Fri, 30 Jun 2023 10:46:08 -0700 -- This is in preparation for deleting imports from the python init file. (`ee6c95cb`)

**Author:** Unknown

**Files touched:**
- `sonnet/src/recurrent.py`

**Commit message:**
```
This is in preparation for deleting imports from the python init file.

```

**Diff:**
```diff
PiperOrigin-RevId: 544696304
---
 sonnet/src/recurrent.py | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/sonnet/src/recurrent.py b/sonnet/src/recurrent.py
index 2c5364dc..97b75b77 100644
--- a/sonnet/src/recurrent.py
+++ b/sonnet/src/recurrent.py
@@ -33,7 +33,7 @@
 
 # pylint: disable=g-direct-tensorflow-import
 # Required for specializing `UnrolledLSTM` per device.
-from tensorflow.python import context as context_lib
+from tensorflow.python.eager import context as context_lib
 # pylint: enable=g-direct-tensorflow-import
 
 



```

---

## Thu, 23 Feb 2023 07:38:33 -0800 -- PiperOrigin-RevId: 511782775 (`26a685e0`)

**Author:** Unknown

**Files touched:**
- `requirements-tf.txt`
- `sonnet/src/recurrent.py`

**Commit message:**
```
PiperOrigin-RevId: 511782775

```

**Diff:**
```diff
---
 requirements-tf.txt     | 2 +-
 sonnet/src/recurrent.py | 5 ++---
 2 files changed, 3 insertions(+), 4 deletions(-)

diff --git a/requirements-tf.txt b/requirements-tf.txt
index e63489c0..5ca90d45 100644
--- a/requirements-tf.txt
+++ b/requirements-tf.txt
@@ -1,2 +1,2 @@
-tensorflow==2.11.0
+tensorflow==2.12.0rc0
 tensorflow-probability==0.12.2
diff --git a/sonnet/src/recurrent.py b/sonnet/src/recurrent.py
index 6c2023d7..2c5364dc 100644
--- a/sonnet/src/recurrent.py
+++ b/sonnet/src/recurrent.py
@@ -34,7 +34,6 @@
 # pylint: disable=g-direct-tensorflow-import
 # Required for specializing `UnrolledLSTM` per device.
 from tensorflow.python import context as context_lib
-from tensorflow.python.eager import function as function_lib
 # pylint: enable=g-direct-tensorflow-import
 
 
@@ -1029,9 +1028,9 @@ def wrapper(*args, **kwargs):
     unique_api_name = "{}_{}".format(api_name, uuid.uuid4())
     functions = {}
     for device, specialization in specializations.items():
-      functions[device] = function_lib.defun_with_attributes(
+      functions[device] = tf.function(
           specialization,
-          attributes={
+          experimental_attributes={
               "api_implements": unique_api_name,
               "api_preferred_device": device
           })



```

---

## Thu, 23 Feb 2023 04:13:10 -0800 -- PiperOrigin-RevId: 511748287 (`ee36d849`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/ci.yml`

**Commit message:**
```
PiperOrigin-RevId: 511748287

```

**Diff:**
```diff
---
 .github/workflows/ci.yml | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/.github/workflows/ci.yml b/.github/workflows/ci.yml
index a857f1a3..d083a87c 100644
--- a/.github/workflows/ci.yml
+++ b/.github/workflows/ci.yml
@@ -14,7 +14,7 @@ jobs:
     runs-on: "${{ matrix.os }}"
     strategy:
       matrix:
-        python-version: [3.7, 3.8, 3.9, '3.10']
+        python-version: [3.8, 3.9, '3.10']
         os: [ubuntu-latest]
     steps:
     - uses: actions/checkout@v2



```

---

## Thu, 23 Feb 2023 03:44:04 -0800 -- PiperOrigin-RevId: 511743612 (`384eae34`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/ci.yml`
- `requirements-tf.txt`
- `setup.py`

**Commit message:**
```
PiperOrigin-RevId: 511743612

```

**Diff:**
```diff
---
 .github/workflows/ci.yml | 2 +-
 requirements-tf.txt      | 2 +-
 setup.py                 | 5 +++--
 3 files changed, 5 insertions(+), 4 deletions(-)

diff --git a/.github/workflows/ci.yml b/.github/workflows/ci.yml
index 9012eb51..a857f1a3 100644
--- a/.github/workflows/ci.yml
+++ b/.github/workflows/ci.yml
@@ -14,7 +14,7 @@ jobs:
     runs-on: "${{ matrix.os }}"
     strategy:
       matrix:
-        python-version: [3.7, 3.8]
+        python-version: [3.7, 3.8, 3.9, '3.10']
         os: [ubuntu-latest]
     steps:
     - uses: actions/checkout@v2
diff --git a/requirements-tf.txt b/requirements-tf.txt
index 388a20d0..e63489c0 100644
--- a/requirements-tf.txt
+++ b/requirements-tf.txt
@@ -1,2 +1,2 @@
-tensorflow==2.5.1
+tensorflow==2.11.0
 tensorflow-probability==0.12.2
diff --git a/setup.py b/setup.py
index 09e6bb21..17bb00ec 100644
--- a/setup.py
+++ b/setup.py
@@ -53,8 +53,9 @@ def _parse_requirements(requirements_txt_path):
         'Intended Audience :: Science/Research',
         'License :: OSI Approved :: Apache Software License',
         'Programming Language :: Python :: 3',
-        'Programming Language :: Python :: 3.6',
-        'Programming Language :: Python :: 3.7',
+        'Programming Language :: Python :: 3.8',
+        'Programming Language :: Python :: 3.9',
+        'Programming Language :: Python :: 3.10',
         'Topic :: Scientific/Engineering :: Mathematics',
         'Topic :: Software Development :: Libraries :: Python Modules',
         'Topic :: Software Development :: Libraries',



```

---

## Thu, 15 Dec 2022 07:11:22 -0800 -- PiperOrigin-RevId: 495584613 (`bb650b9f`)

**Author:** Unknown

**Files touched:**
- `sonnet/__init__.py`

**Commit message:**
```
PiperOrigin-RevId: 495584613

```

**Diff:**
```diff
---
 sonnet/__init__.py | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/sonnet/__init__.py b/sonnet/__init__.py
index 828aa3d7..b2fbf495 100644
--- a/sonnet/__init__.py
+++ b/sonnet/__init__.py
@@ -147,7 +147,7 @@
     "static_unroll",
 )
 
-__version__ = "2.0.1"
+__version__ = "2.0.2.dev"
 
 #  ________________________________________
 # / Please don't use symbols in `src` they \



```

---

## Thu, 15 Dec 2022 06:48:59 -0800 -- PiperOrigin-RevId: 495579932 (`950ecb1f`)

**Author:** Unknown

**Files touched:**
- `sonnet/__init__.py`

**Commit message:**
```
PiperOrigin-RevId: 495579932

```

**Diff:**
```diff
---
 sonnet/__init__.py | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/sonnet/__init__.py b/sonnet/__init__.py
index ddb9f03b..828aa3d7 100644
--- a/sonnet/__init__.py
+++ b/sonnet/__init__.py
@@ -147,7 +147,7 @@
     "static_unroll",
 )
 
-__version__ = "2.0.1.dev"
+__version__ = "2.0.1"
 
 #  ________________________________________
 # / Please don't use symbols in `src` they \



```

---

## 2022-11-02T20:49:33Z -- Remove functools.wraps for unbound methods to support inspect.signature (`45c47e65`)

**Author:** Faizan Muhammad <faizanmuhammad@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove functools.wraps for unbound methods to support inspect.signature
PiperOrigin-RevId: 485685579
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove functools.wraps for unbound methods to support inspect.signature]


```

---

## 2022-10-18T16:30:31Z -- Removed call to function_lib's register method (`6b90fdba`)

**Author:** Umer Javed <umerjaved@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Removed call to function_lib's register method
PiperOrigin-RevId: 481941823
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Removed call to function_libs register method]


```

---

## 2022-08-23T20:08:23Z -- BUILD cleanup (`7298417d`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
BUILD cleanup
PiperOrigin-RevId: 469537143
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[BUILD cleanup]


```

---

## 2022-08-16T19:22:00Z -- Use legacy keras optimizer to be compatible with an incoming Keras optimizer migration. (`c92ac8eb`)

**Author:** Chen Qian <chenqian@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use legacy keras optimizer to be compatible with an incoming Keras optimizer migration.
PiperOrigin-RevId: 467992914
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use legacy keras optimizer to be compatible with an incoming Keras optimizer migration.]


```

---

## 2022-03-07T21:15:40Z -- Update test for Python 3.9. (`d1cd3711`)

**Author:** Yilei Yang <yileiyang@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update test for Python 3.9.
PiperOrigin-RevId: 433021769
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update test for Python 3.9.]


```

---

## 2022-02-01T18:27:44Z -- Replace np.bool with np.bool_. (`df5d099d`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Replace np.bool with np.bool_.
As of NumPy 1.20, `np.bool` is an alias of builtin type `bool`.  The numpy
scalar type is `np.bool_`.

PiperOrigin-RevId: 425658873
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Replace np.bool with np.bool_.]


```

---

## 2021-12-15T15:35:49Z -- Defer creation of autograph converted functions until usage. (`5cbfdc35`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Defer creation of autograph converted functions until usage.
Some users (e.g. #226) report issues with autograph. While these can't be fixed
by Sonnet, we can defer the creation of autograph converted functions until they
are first used (e.g. until the first time you use batch norm with
`@tf.function`). This enables users to make use of non-autograph requiring
features in Sonnet and may be a sufficient workaround while TF team debug.

Fixes #226.

PiperOrigin-RevId: 416554741
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Defer creation of autograph converted functions until usage.]


```

---

## 2021-12-13T12:51:03Z -- Bump Sonnet version to `2.0.1.dev`. (`958ea1b3`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump Sonnet version to `2.0.1.dev`.
PiperOrigin-RevId: 416012688
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump Sonnet version to 2.0.1.dev.]


```

---

## 2021-11-30T10:17:47Z -- Disable flaky test from CI (`913cbd32`)

**Author:** Mehdi Amini <mehdiamini@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Disable flaky test from CI
PiperOrigin-RevId: 413094278
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Disable flaky test from CI]


```

---

## 2021-10-17T14:26:53Z -- Tag flaky test sonnet/v2/src/conformance:checkpoint_test_gpu as "notap" (`19e238eb`)

**Author:** Mehdi Amini <mehdiamini@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Tag flaky test sonnet/v2/src/conformance:checkpoint_test_gpu as "notap"
PiperOrigin-RevId: 403759548
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Tag flaky test sonnet/v2/src/conformance:checkpoint_test_gpu as notap]


```

---

## 2021-09-08T08:52:39Z -- Merge pull request #214 from deepmind:dependabot/pip/tensorflow-2.5.1 (`892790c2`)

**Author:** Copybara-Service <copybaraservice@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #214 from deepmind:dependabot/pip/tensorflow-2.5.1
PiperOrigin-RevId: 395424166
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 214 from deepmind:dependabot/pip/tensorflow-2.5.1]


```

---

## 2021-09-02T10:03:58Z -- Added __init__.py files to all src subpackages (`0b550ff8`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added __init__.py files to all src subpackages
PiperOrigin-RevId: 394419068
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added __init__.py files to all src subpackages]


```

---

## 2021-08-25T14:45:08Z -- Bump tensorflow from 2.5.0 to 2.5.1 (`57f44623`)

**Author:** dependabot[bot] <dependabotbot@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump tensorflow from 2.5.0 to 2.5.1
Bumps [tensorflow](https://github.com/tensorflow/tensorflow) from 2.5.0 to 2.5.1.
- [Release notes](https://github.com/tensorflow/tensorflow/releases)
- [Changelog](https://github.com/tensorflow/tensorflow/blob/master/RELEASE.md)
- [Commits](https://github.com/tensorflow/tensorflow/compare/v2.5.0...v2.5.1)

---
updated-dependencies:
- dependency-name: tensorflow
dependency-type: direct:production
...

Signed-off-by: dependabot[bot] <support@github.com>
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump tensorflow from 2.5.0 to 2.5.1]


```

---

## 2021-08-13T18:11:51Z -- N/A BUILD file cleanup (`0e25f47f`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
N/A BUILD file cleanup
PiperOrigin-RevId: 390652227
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[N/A BUILD file cleanup]


```

---

## 2021-08-12T15:56:21Z -- Mark embeddings on VectorQuantizerEMA as non-trainable. (`38adf3aa`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Mark embeddings on VectorQuantizerEMA as non-trainable.
This variable is updated as part of the forward pass (during training) and is
not updated by the optimiser. In TF `trainable` is used to indicate variables
that should be managed by an optimizer, so even though we update this during
training and it is a parameter of our model, we should set `trainable=False`.

Fixes #209.

PiperOrigin-RevId: 390382636
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Mark embeddings on VectorQuantizerEMA as non-trainable.]


```

---

## 2021-06-18T19:01:55Z -- Removed six (`c87468e6`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Removed six
Sonnet 2 does not support 2.X, so it makes little sense to keep six around.

PiperOrigin-RevId: 380235488
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Removed six]


```

---

## 2021-06-18T12:46:27Z -- Remove Python 2 supporting code. (`cf153705`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove Python 2 supporting code.
PiperOrigin-RevId: 380168270
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove Python 2 supporting code.]


```

---

## 2021-06-18T09:55:21Z -- Add functional API to Sonnet 2 inspired by JAX [0] and Haiku [1]. (`9bb751ff`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add functional API to Sonnet 2 inspired by JAX [0] and Haiku [1].
[0] https://github.com/google/jax
[1] https://github.com/deepmind/dm-haiku

PiperOrigin-RevId: 380147873
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add functional API to Sonnet 2 inspired by JAX [0] and Haiku [1].]


```

---

## 2021-06-10T10:05:52Z -- Add pytest github action and get tests passing. (`6c2bf5fc`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add pytest github action and get tests passing.
PiperOrigin-RevId: 378611382
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add pytest github action and get tests passing.]


```

---

## 2021-06-09T11:24:02Z -- Relax result checking for FP values. (`1a5a6c4f`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Relax result checking for FP values.
CUDA-11 produces slightly different results and causes the test failure
otherwise.

PiperOrigin-RevId: 378374202
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Relax result checking for FP values.]


```

---

## 2021-06-01T18:28:39Z -- Add missing typing.Optional type annotations to function parameters. (`81baad2d`)

**Author:** Rebecca Chen <rebeccachen@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add missing typing.Optional type annotations to function parameters.
PiperOrigin-RevId: 376879768
Change-Id: I6efe84d5f989d8d5663d7ab6b5b3559fe071ade3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add missing typing.Optional type annotations to function parameters.]


```

---

## 2021-03-15T18:22:54Z -- Fix sonnet/v2/src/nets/dnc:util_test_gpu test (`c9cdbb4b`)

**Author:** Eugene Zhulenev <eugenezhulenev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix sonnet/v2/src/nets/dnc:util_test_gpu test
PiperOrigin-RevId: 362986883
Change-Id: If791985603e97af2f0bca8c2255a2ba995f27b28
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix sonnet/v2/src/nets/dnc:util_test_gpu test]


```

---

## 2021-03-11T14:35:47Z -- Remove tensorflow-gpu from extra requirements (`76f61d5d`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove tensorflow-gpu from extra requirements
Also update dependencies in requirements-tf.txt

PiperOrigin-RevId: 362278786
Change-Id: Ib8943e04c36a7fe31782aecaede8cd98bfd69347
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove tensorflow-gpu from extra requirements]


```

---

## 2021-02-10T05:14:47Z -- Fix the checkpoint_test after fix for bug b/168905859 (`eb06599a`)

**Author:** Isha Arkatkar <ishaarkatkar@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix the checkpoint_test after fix for bug b/168905859
PiperOrigin-RevId: 356659488
Change-Id: Ibd52064ff9c343cf10eee5bb2720e04e10ccb421
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix the checkpoint_test after fix for bug b/168905859]


```

---

## 2020-10-01T10:15:16Z -- Bump tensorflow from 2.2.0 to 2.2.1 (`af3826a3`)

**Author:** dependabot[bot] <dependabotbot@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump tensorflow from 2.2.0 to 2.2.1
Bumps [tensorflow](https://github.com/tensorflow/tensorflow) from 2.2.0 to 2.2.1.
- [Release notes](https://github.com/tensorflow/tensorflow/releases)
- [Changelog](https://github.com/tensorflow/tensorflow/blob/master/RELEASE.md)
- [Commits](https://github.com/tensorflow/tensorflow/compare/v2.2.0...v2.2.1)

Signed-off-by: dependabot[bot] <support@github.com>
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump tensorflow from 2.2.0 to 2.2.1]


```

---

## 2020-09-30T22:58:35Z -- BUILD cleanup (`c3e07d34`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
BUILD cleanup
PiperOrigin-RevId: 334697535
Change-Id: I738c37f5b79f02bd9ab5d0c027858293ffbeb58a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[BUILD cleanup]


```

---

## 2020-09-14T13:55:50Z -- Internal change. (`91a4c09f`)

**Author:** Lena Martens <lenamartens@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 331537710
Change-Id: Ie0374ee84cc781870ad0717b59a6aaacbfa3fc44
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2020-09-09T12:35:53Z -- Internal refactor (`4d16e109`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal refactor
PiperOrigin-RevId: 330704150
Change-Id: Ib372f470428f41b2656a6d680d86bca73b973a30
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal refactor]


```

---

## 2020-09-07T18:51:40Z -- Internal change (`3241e36d`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 330403382
Change-Id: Ie0ea28603cef70e844cc291f9a4e55fc9d4f9be4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2020-09-07T18:38:15Z -- Internal change (`77792eb3`)

**Author:** Malcolm Reynolds <malcolmreynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 330402541
Change-Id: I37a1c7a3cf9f42635138c57ad0eb26dce3d580d3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2020-07-24T07:21:12Z -- Change saved_model_test back to use assertAllClose. (`2827d95f`)

**Author:** Adrian Kuegel <adriankuegel@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change saved_model_test back to use assertAllClose.
We are dealing here with floating point values, so some small errors can
happen.

PiperOrigin-RevId: 322948216
Change-Id: I4dc8e7f728f6e3500bbf5546bbba2b418d14b13f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change saved_model_test back to use assertAllClose.]


```

---

## 2020-07-16T11:11:35Z -- Ensure data dependence on input to saved model. (`74be6317`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure data dependence on input to saved model.
PiperOrigin-RevId: 321539026
Change-Id: If585d4dfeb8d481e966c39721ef41e4cba37799c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure data dependence on input to saved model.]


```

---

## 2020-07-09T14:36:59Z -- Import ABC from collections.abc (`45b2b157`)

**Author:** Karthikeyan Singaravelan <karthikeyansingaravelan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Import ABC from collections.abc

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Import ABC from collections.abc]


```

---

## 2020-07-02T08:50:10Z -- Rename whitelist to allow. (`be8e9eb2`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Rename whitelist to allow.
PiperOrigin-RevId: 319373568
Change-Id: If0a500774fef2a67e7bef4ed8b63f976c8b038e2
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Rename whitelist to allow.]


```

---

## 2020-07-01T17:50:11Z -- Update TPUStrategy symbol in Sonnet. (`089e3b0d`)

**Author:** Ruoxin Sang <ruoxinsang@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update TPUStrategy symbol in Sonnet.
PiperOrigin-RevId: 319255469
Change-Id: I5cf0d3596cc5e2ffffc4b93ae8184fa2d3f43dd6
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update TPUStrategy symbol in Sonnet.]


```

---

## 2020-05-14T13:01:33Z -- Use tfds for Cifar10 in VQ-VAE example, default to EMA and include cell outputs. (`ae34a78b`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use tfds for Cifar10 in VQ-VAE example, default to EMA and include cell outputs.
PiperOrigin-RevId: 311517163
Change-Id: Ie8ebc0f95c14d6e70118f48695be65ad7773a13b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use tfds for Cifar10 in VQ-VAE example default to EMA and include cell outputs.]


```

---

## 2020-05-09T11:52:43Z -- Run tests with TensorFlow 2.2.0. (`59f8a46a`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run tests with TensorFlow 2.2.0.
PiperOrigin-RevId: 310708598
Change-Id: I7f4b141b4354931d6d241762b9419eaed1b46d46
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run tests with TensorFlow 2.2.0.]


```

---

## 2020-05-04T15:17:28Z -- Change BatchApply to accept any callable that returns a tf.Tensor. (`61b97bec`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change BatchApply to accept any callable that returns a tf.Tensor.
PiperOrigin-RevId: 309746386
Change-Id: I6e7331e9e7facd58f8f5e87e1ba6037bd26e0c2c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change BatchApply to accept any callable that returns a tf.Tensor.]


```

---

## 2020-04-24T20:00:57Z -- Use sphinx_autodoc_typehints for python types. (`550d8c6e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use sphinx_autodoc_typehints for python types.
PiperOrigin-RevId: 308309638
Change-Id: Ie8d2adb67ba0d3159dda3ca255b0b02c0a36d64b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use sphinx_autodoc_typehints for python types.]


```

---

## 2020-04-20T13:48:57Z -- Drop usages of `tf.enable_v2_behavior()`, no longer necessary. (`5c5ef16d`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Drop usages of `tf.enable_v2_behavior()`, no longer necessary.
PiperOrigin-RevId: 307389325
Change-Id: Id5d59c4303dd6c10c0328f801051e95de1bdab23
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Drop usages of tf.enable_v2_behavior no longer necessary.]


```

---

## 2020-04-17T18:23:15Z -- Merge pull request #168 from ialong:patch-1 (`46333904`)

**Author:** Copybara-Service <copybaraservice@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #168 from ialong:patch-1
PiperOrigin-RevId: 307079951
Change-Id: Ic91440e5fe76aec3cf46d39212b4dd4155a42589
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 168 from ialong:patch-1]


```

---

## 2020-04-17T08:19:09Z -- Use latest version of Sonnet in colab examples. (`baf0c573`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use latest version of Sonnet in colab examples.
Fixes #169.

PiperOrigin-RevId: 307003064
Change-Id: I17ac33a29c2dd88ff38d2933adf80c04b2fd27c5
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use latest version of Sonnet in colab examples.]


```

---

## 2020-04-15T16:46:32Z -- add float64 test for VQ-VAE (`018b05e9`)

**Author:** Alessandro Davide Ialongo <alessandrodavideialongo@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
add float64 test for VQ-VAE
make sure `VectorQuantizerEMA` works if the dtype is tf.float64
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[add float64 test for VQ-VAE]


```

---

## 2020-04-12T12:31:41Z -- fix type of one hot indicators in vqvae.py (`58d9a274`)

**Author:** Alessandro Davide Ialongo <alessandrodavideialongo@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
fix type of one hot indicators in vqvae.py
`tf.one_hot` defaults to `tf.float32` if no dtype is provided. This prevents `VectorQuantizerEMA` from running if its dtype is specified as `tf.float64` at init time (since the EMA tries to subtract a `tf.float32` - the one hot indicator tensor - from a `tf.float64`). This is easily fixed by passing a dtype argument to `tf.one_hot`.
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[fix type of one hot indicators in vqvae.py]


```

---

## 2020-04-02T14:13:06Z -- Fix for change to conv_transpose (`3b8f9d4c`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix for change to conv_transpose
PiperOrigin-RevId: 304394090
Change-Id: I7cdbfde46fc70a7c324ac8d7b4fc50ef89516639
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix for change to conv_transpose]


```

---

## 2020-03-27T11:08:06Z -- Update README to reflect 2.0 release. (`67a0955c`)

**Author:** John Aslanides <johnaslanides@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update README to reflect 2.0 release.
PiperOrigin-RevId: 303301305
Change-Id: Ifdef58d5cf7170da8b15bccaea7a2b8d692ec1ae
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update README to reflect 2.0 release.]


```

---

## 2020-03-27T10:05:43Z -- Update Sonnet version to 2.0.0. (`940b4d11`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update Sonnet version to 2.0.0.
PiperOrigin-RevId: 303292840
Change-Id: I0118990892ac9c369c94de966a092f5056d1eafd
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update Sonnet version to 2.0.0.]


```

---

## 2020-03-19T23:32:05Z -- Apply name change(experimental_run_v2 -> run) for all callers. (`bb53c637`)

**Author:** Ken Franko <kenfranko@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Apply name change(experimental_run_v2 -> run) for all callers.
PiperOrigin-RevId: 301919882
Change-Id: I14c6ed85bdf50d619d1bc572e0fbcc5f1821c70b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Apply name changeexperimental_run_v2 - run for all callers.]


```

---

## 2020-03-16T12:28:44Z -- Ensure batch norm works for inference when the number of channels is 1 (`de30fdab`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure batch norm works for inference when the number of channels is 1
PiperOrigin-RevId: 301137212
Change-Id: I8c9d03dce81d7b38b2d082873c9fba75662541b3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure batch norm works for inference when the number of channels is 1]


```

---

## 2020-02-27T14:56:39Z -- Fix typing of cifar10_convnet after previous change (`733d6bbb`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix typing of cifar10_convnet after previous change
PiperOrigin-RevId: 297585986
Change-Id: Icdcfbdec09feafb1026385bf794d252a626a8219
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix typing of cifar10_convnet after previous change]


```

---

## 2020-02-26T19:45:26Z -- Explicitly create names for batch_norm moving statistics. (`2a8e1d8e`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Explicitly create names for batch_norm moving statistics.
PiperOrigin-RevId: 297411469
Change-Id: I6c9c37958930614bf47f10edf51dafb161b2ace3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Explicitly create names for batch_norm moving statistics.]


```

---

## 2020-02-26T14:15:04Z -- Fix indentation error (`c8fbd100`)

**Author:** Hanbyul Kim <hanbyulkim@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix indentation error

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix indentation error]


```

---

## 2020-02-26T13:34:00Z -- Remove extra newlines (`e7f7b619`)

**Author:** Hanbyul Kim <hanbyulkim@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove extra newlines

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove extra newlines]


```

---

## 2020-02-26T13:19:04Z -- Fix code block to use :: (`623a88d4`)

**Author:** Hanbyul Kim <hanbyulkim@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix code block to use ::

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix code block to use ::]


```

---

## 2020-02-21T09:25:53Z -- Merge pull request #159 from chris-chris:feature/2002-mac-test (`1f5c0c24`)

**Author:** Sonnet Copybara <sonnetcopybara@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #159 from chris-chris:feature/2002-mac-test
PiperOrigin-RevId: 296389674
Change-Id: Ifce8e017fbbce4617657f958e83667e8d71e51bd
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 159 from chris-chris:feature/2002-mac-test]


```

---

## 2020-02-20T20:09:08Z -- Fixed a few formatting quirks in recurrent docs (`d4f38d4b`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed a few formatting quirks in recurrent docs
PiperOrigin-RevId: 296267592
Change-Id: I9094ae88cf8ef16ee3721c8bd8f19532008a0f13
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed a few formatting quirks in recurrent docs]


```

---

## 2020-02-19T18:05:28Z -- Remove values property from DistributedValues. (`12c3b22c`)

**Author:** Ken Franko <kenfranko@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove values property from DistributedValues.
PiperOrigin-RevId: 295994651
Change-Id: I663f2b2b94a04e4a9ac01ca63629d7bbf412ea15
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove values property from DistributedValues.]


```

---

## 2020-02-18T22:56:03Z -- Make primary property on DistributedValue private. (`b6152b7a`)

**Author:** Ken Franko <kenfranko@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make primary property on DistributedValue private.
PiperOrigin-RevId: 295829087
Change-Id: I5c0bf035780f92de197578c979301ac900dc7afc
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make primary property on DistributedValue private.]


```

---

## 2020-02-10T12:28:06Z -- Make snt.Linear compatible with more than one batch dimension. (`4bbbcbc5`)

**Author:** Mehdi S. M. Sajjadi <mehdismsajjadi@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make snt.Linear compatible with more than one batch dimension.
The Linear layer should be compatible with inputs of dimension 2 or more. All
dimensions but the last one will be treated as batch dimensions.

PiperOrigin-RevId: 294193315
Change-Id: Ia673936f09832dabfaea02e612f34a59c8da187e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make snt.Linear compatible with more than one batch dimension.]


```

---

## 2020-01-30T14:18:49Z -- Merge pull request #157 from deepmind:dependabot/pip/tensorflow-2.0.1 (`f9cc3e9a`)

**Author:** Sonnet Copybara <sonnetcopybara@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #157 from deepmind:dependabot/pip/tensorflow-2.0.1
PiperOrigin-RevId: 292334684
Change-Id: Ib2fff7befb0c83110abe292dcdfbe144a549f41b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 157 from deepmind:dependabot/pip/tensorflow-2.0.1]


```

---

## 2020-01-28T21:46:22Z -- Bump tensorflow from 2.0.0 to 2.0.1 (`9eaac574`)

**Author:** dependabot[bot] <dependabotbot@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump tensorflow from 2.0.0 to 2.0.1
Bumps [tensorflow](https://github.com/tensorflow/tensorflow) from 2.0.0 to 2.0.1.
- [Release notes](https://github.com/tensorflow/tensorflow/releases)
- [Changelog](https://github.com/tensorflow/tensorflow/blob/master/RELEASE.md)
- [Commits](https://github.com/tensorflow/tensorflow/compare/v2.0.0...v2.0.1)

Signed-off-by: dependabot[bot] <support@github.com>
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump tensorflow from 2.0.0 to 2.0.1]


```

---

## 2020-01-27T14:23:29Z -- Fuzz tests for more optimizers. (`f18c55e3`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fuzz tests for more optimizers.
PiperOrigin-RevId: 291709836
Change-Id: Ia1282ba02649863e1ea39c481a84e31f68ace7ea
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fuzz tests for more optimizers.]


```

---

## 2020-01-27T09:46:12Z -- Merge pull request #155 from fostiropoulos:v2 (`4fe2373b`)

**Author:** Sonnet Copybara <sonnetcopybara@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #155 from fostiropoulos:v2
PiperOrigin-RevId: 291678292
Change-Id: I390276d492db8e7b1aaaf994b9e8592391d2e7ad
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 155 from fostiropoulos:v2]


```

---

## 2020-01-25T19:48:24Z -- Fixed vqvae_example.ipynb to be compatible with tf v2 (`82049c26`)

**Author:** fostiropoulos <fostiropoulos@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed vqvae_example.ipynb to be compatible with tf v2

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed vqvae_example.ipynb to be compatible with tf v2]


```

---

## 2020-01-22T09:52:41Z -- Shard conformance tests to allow faster testing. (`af606a45`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Shard conformance tests to allow faster testing.
PiperOrigin-RevId: 290911624
Change-Id: Iacf31a4798c428942708acc43ecf0d9f071ce88c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Shard conformance tests to allow faster testing.]


```

---

## 2020-01-19T13:16:26Z -- Create `snt.distribute.create_variables_eagerly(f)`. (`f44c1330`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Create `snt.distribute.create_variables_eagerly(f)`.
`create_variables_eagerly` is a higher order function that attempts to lift all
variable creation within a given function into eager mode. It does this by
patching Sonnet initializers to run under a `tf.init_scope`, and then patching
variable creation to also run under a `tf.init_scope`. Additionally we support
statically determining some values not created using Sonnet (e.g. `tf.zeros`).

This function is useful when doing large scale distributed training with
TensorFlow on clusters of machines. We can avoid much of the checking that
happens for variables created in tf.functions which result in a lot of RPCs
between the coordinator and workers, slowing down the first training step.

PiperOrigin-RevId: 290499884
Change-Id: I1fce83011df198ff23c044689b6e5d0e60094be2
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Create snt.distribute.create_variables_eagerlyf.]


```

---

## 2020-01-17T10:33:27Z -- Fix bug in _rnn_step, by swapping the prev_state with state. (`25f040a2`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix bug in _rnn_step, by swapping the prev_state with state.
PiperOrigin-RevId: 290235436
Change-Id: Iea5fb1d0b6a14e6f050212293861b9fc107b7e9c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix bug in _rnn_step by swapping the prev_state with state.]


```

---

## 2020-01-08T13:04:39Z -- Expose block group and bottle neck blocks from resnet. (`0c623c0c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose block group and bottle neck blocks from resnet.
PiperOrigin-RevId: 288675258
Change-Id: Id3eb4de377f09a497fb5cfd680d7f898e4413e3a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose block group and bottle neck blocks from resnet.]


```

---

## 2020-01-07T15:20:02Z -- Add check on inputs and outputs for TPU strategy test (`431c6e4c`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add check on inputs and outputs for TPU strategy test
PiperOrigin-RevId: 288491883
Change-Id: I6f1b8db69ca360d152bf6eeb21bf1c06aeedbf92
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add check on inputs and outputs for TPU strategy test]


```

---

## 2020-01-06T16:54:31Z -- Add cross-replica BatchNorm test on TPU strategy. (`6e5de35a`)

**Author:** Ken Franko <kenfranko@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add cross-replica BatchNorm test on TPU strategy.
PiperOrigin-RevId: 288309269
Change-Id: Ibe831d68d273fd9d279e663625d1fed7e84bba46
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add cross-replica BatchNorm test on TPU strategy.]


```

---

## 2020-01-02T18:20:12Z -- Remove eager variable creation hack. (`1d16fc10`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove eager variable creation hack.
Underlying performance issue has been fixed.  Workaround no longer needed.

PiperOrigin-RevId: 287854778
Change-Id: Idc9609bf73baa432700bc2fe69dafefdafd970c6
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove eager variable creation hack.]


```

---

## 2020-01-02T09:05:41Z -- Enable ResNet50 V2, has reproduced canonical experiment. (`361f0335`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Enable ResNet50 V2, has reproduced canonical experiment.
PiperOrigin-RevId: 287803490
Change-Id: Id59f82145be188d1f587602d604c5aa12e9f5e2d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Enable ResNet50 V2 has reproduced canonical experiment.]


```

---

## 2019-12-18T10:34:29Z -- Removed wrapt.ObjectProxy hack from TrainableState (`d7f19980`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Removed wrapt.ObjectProxy hack from TrainableState
tree now supports wrapped objects in the same way as tf.nest

PiperOrigin-RevId: 286154954
Change-Id: I35dfeedf96be5d85bb1b74e2d79961e91172175b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Removed wrapt.ObjectProxy hack from TrainableState]


```

---

## 2019-12-16T13:39:05Z -- Migrated from tf.nest to tree (`6c6c533b`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Migrated from tf.nest to tree
PiperOrigin-RevId: 285753613
Change-Id: I444db9927b81e04fb46a55edf44705fd2d511aad
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Migrated from tf.nest to tree]


```

---

## 2019-12-10T12:23:03Z -- Fix or ignore type errors generated by the next release of pytype. (`84817e56`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix or ignore type errors generated by the next release of pytype.
The next pytype release includes better support for quoted annotations, which
reveals type errors that were previously hidden due to pytype occasionally
treating such annotations as Any rather than the quoted type.

PiperOrigin-RevId: 284742182
Change-Id: Iba35edc283680295da815ee3c74a48eb67d158a0
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix or ignore type errors generated by the next release of pytype.]


```

---

## 2019-12-04T16:48:01Z -- Drop bayes_by_backprop.py and remove dependency on tfp. (`2ef5c4fd`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Drop bayes_by_backprop.py and remove dependency on tfp.
PiperOrigin-RevId: 283764831
Change-Id: If8704c77238c4cc2b3650d58559d7ff833d44536
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Drop bayes_by_backprop.py and remove dependency on tfp.]


```

---

## 2019-11-15T14:45:02Z -- Minor fixes. (`50413fe1`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Minor fixes.
PiperOrigin-RevId: 280648498
Change-Id: I117af823bf44b3c58c20639008656d128a75891e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Minor fixes.]


```

---

## 2019-11-06T11:45:07Z -- VQVAE example notebook ported to Sonnet 2. (`d8557c39`)

**Author:** Malcolm Reynolds <malcolmreynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
VQVAE example notebook ported to Sonnet 2.
PiperOrigin-RevId: 278825348
Change-Id: I341cb0ee0a01d0ec93ba477a4489bc9ef3883969
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[VQVAE example notebook ported to Sonnet 2.]


```

---

## 2019-11-04T18:45:33Z -- Re-add eager variable creation hack for TPUs. (`f9cbc4f1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Re-add eager variable creation hack for TPUs.
PiperOrigin-RevId: 278415088
Change-Id: I8cede4c69ebfd37fb606614647dccba1e7de49b9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Re-add eager variable creation hack for TPUs.]


```

---

## 2019-11-04T13:02:06Z -- Fix bug in Conv*Transpose weight initialization (`605ca701`)

**Author:** Malcolm Reynolds <malcolmreynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix bug in Conv*Transpose weight initialization
PiperOrigin-RevId: 278353842
Change-Id: I8321c3488efd49304c3efe6a05e319217a80903f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix bug in ConvTranspose weight initialization]


```

---

## 2019-11-01T16:33:35Z -- Align names and defaults with Adam paper, add reference. (`e3ad61e0`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Align names and defaults with Adam paper, add reference.
PiperOrigin-RevId: 277942532
Change-Id: Ie660aae26b25a74ff55f06bf7bfb12790dcde80a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Align names and defaults with Adam paper add reference.]


```

---

## 2019-10-30T17:50:12Z -- Remove eager variable creation logic and tests from `replicator.py`. (`c787becf`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove eager variable creation logic and tests from `replicator.py`.
PiperOrigin-RevId: 277538633
Change-Id: I92b4568094893b7ee50a5765218f629de7fac700
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove eager variable creation logic and tests from replicator.py.]


```

---

## 2019-10-30T16:39:59Z -- Support restoring on creation for Sonnet model with TPUReplicator. (`790bd159`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support restoring on creation for Sonnet model with TPUReplicator.
PiperOrigin-RevId: 277523053
Change-Id: I2507f5ea24214c7e7465bb2ca6fb692758321382
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support restoring on creation for Sonnet model with TPUReplicator.]


```

---

## 2019-10-30T10:59:07Z -- Make `BayesByBackprop` a Sonnet module. (`61cd600a`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make `BayesByBackprop` a Sonnet module.
PiperOrigin-RevId: 277475497
Change-Id: Iafe3469bbb6a1ed2a28a4365491306d96b9c1d56
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make BayesByBackprop a Sonnet module.]


```

---

## 2019-10-25T08:57:56Z -- Increase tolerance to fix test flakiness. (`feb7f7d9`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Increase tolerance to fix test flakiness.
PiperOrigin-RevId: 276652089
Change-Id: I88f53aea3f7c06169df5f37245489b637c60e477
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Increase tolerance to fix test flakiness.]


```

---

## 2019-10-23T19:28:49Z -- Remove internal tag. (`d569c7e1`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove internal tag.
PiperOrigin-RevId: 276327844
Change-Id: Id336713171bb98f82400829491bee66f8843c7dc
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove internal tag.]


```

---

## 2019-10-23T16:52:55Z -- Remove unnecessary build dependencies. (`41236fb9`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove unnecessary build dependencies.
PiperOrigin-RevId: 276294400
Change-Id: Ica20bb758cd1a84df922053de734120292e07a3c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove unnecessary build dependencies.]


```

---

## 2019-10-23T14:36:04Z -- Move load statements to top of BUILD file. (`e6ce4bd4`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move load statements to top of BUILD file.
PiperOrigin-RevId: 276271417
Change-Id: I15835a51624df09a4126c4e98ab365da3244d0e2
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move load statements to top of BUILD file.]


```

---

## 2019-10-23T13:48:26Z -- Move optimizers to a subdir so the src better reflects the API structure. (`5ab2b334`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move optimizers to a subdir so the src better reflects the API structure.
PiperOrigin-RevId: 276265404
Change-Id: I7de7ced4b41f142c34fd7812e755d30643cefef6
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move optimizers to a subdir so the src better reflects the API structure.]


```

---

## 2019-10-22T22:30:59Z -- Improve docstring for optimizer module. (`323a220a`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve docstring for optimizer module.
PiperOrigin-RevId: 276157441
Change-Id: I5cac879788ecb83f8cab12b1e8da8839b894e97e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve docstring for optimizer module.]


```

---

## 2019-10-18T06:27:51Z -- Remove raw_ops optimizers and test against tf.optimizers instead. (`d87e60d2`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove raw_ops optimizers and test against tf.optimizers instead.
We need to skip a bunch of tests in the base class because they don't apply to
the builtin TF optimizers. I think there's an opportunity to clean this up a
little but I wanted to minimize the delta in this change so will follow up.

PiperOrigin-RevId: 275413174
Change-Id: I67bec8deced90b9923c37828a107917a57a0de71
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove raw_ops optimizers and test against tf.optimizers instead.]


```

---

## 2019-10-16T19:50:47Z -- Unify sparse and dense update rules into pure functional versions. (`484ffb3e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Unify sparse and dense update rules into pure functional versions.
PiperOrigin-RevId: 275093209
Change-Id: I1b2852bb8b9c77cc8c70bc2be1e1dd6d63a7fd16
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Unify sparse and dense update rules into pure functional versions.]


```

---

## 2019-10-16T15:03:52Z -- Use sync-on-read variables for TPU strategy now these have been fixed (`7fcf1888`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use sync-on-read variables for TPU strategy now these have been fixed
PiperOrigin-RevId: 275033031
Change-Id: I1bf8dd0cda187446003828f35f31016ae4191c95
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use sync-on-read variables for TPU strategy now these have been fixed]


```

---

## 2019-10-11T19:31:55Z -- Fixed cases where tf.TensorShape was constructed with float dimensions (`3d0fffe2`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed cases where tf.TensorShape was constructed with float dimensions
This is a prerequisite for making TensorShape and Dimension more strict
about the types of their arguments.

PiperOrigin-RevId: 274225941
Change-Id: Id2be16daabf8e3770d23778c595073509714c92a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed cases where tf.TensorShape was constructed with float dimensions]


```

---

## 2019-10-11T10:40:12Z -- Change snt.net.ResNet documentation to match constructor. (`1833bf69`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change snt.net.ResNet documentation to match constructor.
Fixes #150

PiperOrigin-RevId: 274142117
Change-Id: I6dc274045f52ca21005532d948610dd75e08f47b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change snt.net.ResNet documentation to match constructor.]


```

---

## 2019-10-11T10:25:01Z -- Ensure all test in Sonnet are run when test.sh is called and fixes to enable this (`275af721`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure all test in Sonnet are run when test.sh is called and fixes to enable this
Also remove the dataset fetching test from within examples as we shouldn't be reading from/downloading files within tests

PiperOrigin-RevId: 274140680
Change-Id: I872e4251a1a8815afbe566c3499986bb3be31429
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure all test in Sonnet are run when test.sh is called and fixes to enable this]


```

---

## 2019-10-08T13:31:52Z -- UnrolledLSTM now checks for GPU availability to choose specialization in eager mode (`6725d73f`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
UnrolledLSTM now checks for GPU availability to choose specialization in eager mode
Prior to this change

lstm = UnrolledLSTM(...)
lstm(...)

was always executed using the default specialization. Now the specialization is chosen
based on the availability of GPU, so if a GPU is present, UnrolledLSTM will use
the CuDNN-RNN specialization.

PiperOrigin-RevId: 273506804
Change-Id: I55cac6dd568ab4f6c24ce38cf830ce978305c6c2
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[UnrolledLSTM now checks for GPU availability to choose specialization in eager mode]


```

---

## 2019-10-08T08:15:32Z -- Test variables that are deep copied have the same name. (`a6ddb71c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test variables that are deep copied have the same name.
PiperOrigin-RevId: 273468646
Change-Id: I9b788823e833a4a4fd52697c878e938d6a527450
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test variables that are deep copied have the same name.]


```

---

## 2019-10-07T15:00:45Z -- Avoid throwing exception when comparing ndarray to default values. (`69b02ac3`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Avoid throwing exception when comparing ndarray to default values.
PiperOrigin-RevId: 273291764
Change-Id: Ia35464442d30b7f36d0cd71233b5e93114644e51
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Avoid throwing exception when comparing ndarray to default values.]


```

---

## 2019-10-06T10:53:26Z -- Make Sonnet beta label more obvious. (`3ca76553`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make Sonnet beta label more obvious.
PiperOrigin-RevId: 273141185
Change-Id: If0defb92dbe706244c4b9b37d981737cbf3b7e6d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make Sonnet beta label more obvious.]


```

---

## 2019-10-06T10:16:31Z -- Make TF-Probability a non-optional requirement. (`a44b88d1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make TF-Probability a non-optional requirement.
PiperOrigin-RevId: 273138831
Change-Id: I1a6be962c77e2003689848564ca7c78bc60260d9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make TF-Probability a non-optional requirement.]


```

---

## 2019-10-05T09:06:33Z -- Remove default values for arguments in auto_repr (even if supplied). (`79dd65b0`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove default values for arguments in auto_repr (even if supplied).
PiperOrigin-RevId: 273035791
Change-Id: Idbec600a5dc02172dcd1ac2562f706b7815ebad9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove default values for arguments in auto_repr even if supplied.]


```

---

## 2019-10-04T13:45:59Z -- Include details about which module doesn't contain variables. (`45e0801d`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Include details about which module doesn't contain variables.
Example error:

```
>>> m = snt.nets.ResNet50(10)
>>> m.trainable_variables
...
ValueError: ResNet50(num_classes=10) does not currently contain any trainable_variables.

Most Sonnet modules create variables the first time they are called with an
input and requesting variables before this typically indicates a coding error.

You should refactor your code such that you request module variables after you
pass an example input to the module. For example:

module = ResNet50(num_classes=10)
output = module(input)
params = module.trainable_variables

If the module is stateless consider using `snt.allow_empty_variables(module)` to
suppress this error:

module = ResNet50(num_classes=10)
snt.allow_empty_variables(module)
params = module.trainable_variables
```

PiperOrigin-RevId: 272866557
Change-Id: I4547a36acdfe9afa17ee13ed09dc24fddbce1890
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Include details about which module doesnt contain variables.]


```

---

## 2019-10-04T08:54:27Z -- Pin tensorflow-probability @ 0.8.0. (`87591dfe`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Pin tensorflow-probability @ 0.8.0.
PiperOrigin-RevId: 272832330
Change-Id: I590b78cbc9078eb1711df17fc18c77307d848fd0
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Pin tensorflow-probability  0.8.0.]


```

---

## 2019-10-03T13:26:17Z -- Assert that we are running with TF2 when modules are created. (`d7c0f5f5`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Assert that we are running with TF2 when modules are created.
PiperOrigin-RevId: 272647398
Change-Id: I8e7f0cdaa7c5f2efe6ebec8e2e29632c3383994d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Assert that we are running with TF2 when modules are created.]


```

---

## 2019-10-03T09:36:17Z -- Test module methods with @tf.custom_gradient. (`0c159694`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test module methods with @tf.custom_gradient.
PiperOrigin-RevId: 272619808
Change-Id: I43606ef4a10a02ecf0043ad67ff0b0326d49870c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test module methods with tf.custom_gradient.]


```

---

## 2019-10-03T09:31:07Z -- Only doctest classes or objects (e.g. functions) with __doc__. (`b3499f96`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Only doctest classes or objects (e.g. functions) with __doc__.
PiperOrigin-RevId: 272619129
Change-Id: I66caf524fa8c15951d721ae619a08a9d41898aad
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Only doctest classes or objects e.g. functions with __doc__.]


```

---

## 2019-10-02T16:36:50Z -- Test all Sonnet modules with `copy.deepcopy`. (`03e17a3c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test all Sonnet modules with `copy.deepcopy`.
PiperOrigin-RevId: 272455764
Change-Id: I8de3d4a78550ce737844580b0ba118d3eee76397
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test all Sonnet modules with copy.deepcopy.]


```

---

## 2019-10-01T15:38:01Z -- Explicitly don't implement to/from config in LayerAdapter. (`6740f601`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Explicitly don't implement to/from config in LayerAdapter.
PiperOrigin-RevId: 272218220
Change-Id: I195209f8ae19aa7620b21c687afade19bae881d3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Explicitly dont implement to/from config in LayerAdapter.]


```

---

## 2019-10-01T14:58:16Z -- Fix ResNet50 docstring. (`220c87d7`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix ResNet50 docstring.
resnet_v2 is set to `False` by default but described as `True` in docstring.

PiperOrigin-RevId: 272209667
Change-Id: Ia11ada33e5853dbe22ca2ad962bcc4eb4e84e924
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix ResNet50 docstring.]


```

---

## 2019-10-01T11:10:23Z -- Consistently reference TF2 stable release in docs. (`6f45ea8e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Consistently reference TF2 stable release in docs.
PiperOrigin-RevId: 272178930
Change-Id: I2330553d8891693f067a8876644bf65beaf2dcfb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Consistently reference TF2 stable release in docs.]


```

---

## 2019-10-01T07:26:25Z -- Pin to TensorFlow 2.0.0, no longer pin gast (TF does this itself). (`d96af1fb`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Pin to TensorFlow 2.0.0, no longer pin gast (TF does this itself).
$ johnnydep tensorflow 2>&1 | grep gast
2019-10-01 06:23:58 [info     ] init johnnydist                [johnnydep.lib] dist=gast==0.2.2 parent=tensorflow

PiperOrigin-RevId: 272149530
Change-Id: Iad9bed9ae2b9dc47c0c71eee67ac13b54208888a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Pin to TensorFlow 2.0.0 no longer pin gast TF does this itself.]


```

---

## 2019-09-30T16:45:45Z -- Export CrossReplicaBatchNorm (`544f5582`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Export CrossReplicaBatchNorm
PiperOrigin-RevId: 272002620
Change-Id: I832632f05d72beb259421b3948ab27a2c5be7225
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Export CrossReplicaBatchNorm]


```

---

## 2019-09-30T12:08:54Z -- Add (undocumented) option to disable name scopes process wide. (`f096923d`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add (undocumented) option to disable name scopes process wide.
When profiling it can be frustrating to see two "trampoline" methods before user
code (_decorate_unbound_method -> wrap_with_name_scope). This environment
variable disables name scoping (and thus those trampolines) everywhere apart
from `__init__`.

Note that you probably only want to enable this environment variable once you
have ruled out that the overhead (entering and exiting a `tf.name_scope`) from
the trampoline methods is not significant for your problem.

PiperOrigin-RevId: 271959140
Change-Id: I826aa9cb9e8c4a5a6584afff5a6567c456446afb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add undocumented option to disable name scopes process wide.]


```

---

## 2019-09-28T20:57:35Z -- TFDS: specify explicit version of dataset being used for reproducibility. (`080efa93`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
TFDS: specify explicit version of dataset being used for reproducibility.
PiperOrigin-RevId: 271772074
Change-Id: I74a6608bebadfeac9684cbd3222fd8d9fd627ee9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[TFDS: specify explicit version of dataset being used for reproducibility.]


```

---

## 2019-09-27T10:55:56Z -- Add test for simple_mnist example (`a4dd8093`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add test for simple_mnist example
PiperOrigin-RevId: 271541189
Change-Id: I6aff015b6c6ddf097b47a65fecd855ba59ac8a10
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add test for simple_mnist example]


```

---

## 2019-09-26T08:05:58Z -- Add tests for unrolling RNN cores with TpuReplicator. (`8e6749b1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add tests for unrolling RNN cores with TpuReplicator.
PiperOrigin-RevId: 271297102
Change-Id: I455a7e8c5af623ec66c40ac07ee5fa310485e89e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add tests for unrolling RNN cores with TpuReplicator.]


```

---

## 2019-09-23T13:33:03Z -- Set setup.py description content type to Markdown. (`ff190246`)

**Author:** Diego de Las Casas <diegodelascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Set setup.py description content type to Markdown.
PiperOrigin-RevId: 270663464
Change-Id: Ib5693cb3690e374617c08ef800265882f3fd9afb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Set setup.py description content type to Markdown.]


```

---

## 2019-09-23T13:13:27Z -- Merge pull request #147 from tomhennigan:v2 (`13039b6d`)

**Author:** Sonnet Copybara <sonnetcopybara@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #147 from tomhennigan:v2
PiperOrigin-RevId: 270660761
Change-Id: Ice9fcf7325ba4a8b016ed94566ebb3d81e572cbc
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 147 from tomhennigan:v2]


```

---

## 2019-09-21T12:56:46Z -- Add virtual GPUs and improve accuracy in m-gpu example. (`48de820e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add virtual GPUs and improve accuracy in m-gpu example.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add virtual GPUs and improve accuracy in m-gpu example.]


```

---

## 2019-09-21T11:55:35Z -- Make use of `%tensorflow_version`. (`c440bcf1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make use of `%tensorflow_version`.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make use of tensorflow_version.]


```

---

## 2019-09-21T11:55:23Z -- Make use of `%tensorflow_version`. (`6006916f`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make use of `%tensorflow_version`.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make use of tensorflow_version.]


```

---

## 2019-09-20T11:03:24Z -- Add type annotatinos to simple_mnist example. (`7c31fb5e`)

**Author:** John Aslanides <johnaslanides@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add type annotatinos to simple_mnist example.
PiperOrigin-RevId: 270239254
Change-Id: I5cbdb606a63db9820ed1e890a9f7bf0668637401
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add type annotatinos to simple_mnist example.]


```

---

## 2019-09-19T18:57:36Z -- Create `snt.build` which triggers variable creation on modules. (`dd5da184`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Create `snt.build` which triggers variable creation on modules.
>>> mod = snt.nets.MLP([1000, 10])
>>> snt.build(mod, [None, 28 * 28])
TensorSpec(shape=(None, 10), dtype=tf.float32, name=None)
>>> mod.variables
(<tf.Variable 'mlp/linear_0/b:0' shape=(1000,) ...>,
<tf.Variable 'mlp/linear_0/w:0' shape=(784, 1000) ...>,
<tf.Variable 'mlp/linear_1/b:0' shape=(10,) ...>,
<tf.Variable 'mlp/linear_1/w:0' shape=(1000, 10) ...>)

PiperOrigin-RevId: 270093041
Change-Id: Ieee5967d96afb623f7d485176b47dfb4f8a7d944
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Create snt.build which triggers variable creation on modules.]


```

---

## 2019-09-19T18:30:47Z -- Fix reset of vector valued moving_average. (`0ef0a79e`)

**Author:** Zafarali Ahmed <zafaraliahmed@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix reset of vector valued moving_average.
PiperOrigin-RevId: 270087520
Change-Id: I0cbe41dfd5b7c7cbab450117512926b88170623a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix reset of vector valued moving_average.]


```

---

## 2019-09-19T14:05:39Z -- Run yapf (github.com/google/yapf) over source files. (`f12fec44`)

**Author:** John Aslanides <johnaslanides@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run yapf (github.com/google/yapf) over source files.
PiperOrigin-RevId: 270032571
Change-Id: I3c68dbbbdb95eab950ab36d5dceece14c1b2616c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run yapf github.com/google/yapf over source files.]


```

---

## 2019-09-19T08:39:59Z -- Add elided words to EMA docstring (`7f37d695`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add elided words to EMA docstring
PiperOrigin-RevId: 269986522
Change-Id: I562dfba05ff325bd9bfc65c801c7a7a4ea3f45d1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add elided words to EMA docstring]


```

---

## 2019-09-18T16:51:01Z -- Add workaround for slow startup time on TPUs. (`bde4d188`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add workaround for slow startup time on TPUs.
This takes ResNet50 startup from 42m -> 2m on a 4x4.

PiperOrigin-RevId: 269828188
Change-Id: I0dfbd6a4224e4df71267a86e3a82a2a1dcb2914c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add workaround for slow startup time on TPUs.]


```

---

## 2019-09-17T20:47:37Z -- Use latest versions of TF and Sonnet in MLP example. (`ddeb644a`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use latest versions of TF and Sonnet in MLP example.
PiperOrigin-RevId: 269646018
Change-Id: Iea0b70d63575ade0e74ec8ce3ee74b682b345c9a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use latest versions of TF and Sonnet in MLP example.]


```

---

## 2019-09-17T16:01:27Z -- Add type annotations to linear and MLP. (`1de58ba8`)

**Author:** John Aslanides <johnaslanides@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add type annotations to linear and MLP.
PiperOrigin-RevId: 269580438
Change-Id: I6bcf37b35988d063ff9072404918005d62cc715e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add type annotations to linear and MLP.]


```

---

## 2019-09-17T12:46:53Z -- Use tabulate to format variables table. (`ac2f0b37`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use tabulate to format variables table.
PiperOrigin-RevId: 269549068
Change-Id: I998ded3cbbe315ad233fd2578c3d9c53a505bd4c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use tabulate to format variables table.]


```

---

## 2019-09-17T09:56:04Z -- Allow lowercase padding names in Conv*D. (`febdafe9`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow lowercase padding names in Conv*D.
PiperOrigin-RevId: 269528598
Change-Id: I7f186ba669422ce0d8d8665403909c0e56f6d3cf
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow lowercase padding names in ConvD.]


```

---

## 2019-09-16T21:47:22Z -- Minor changes to little GAN colab, including cell outputs. (`96ec0479`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Minor changes to little GAN colab, including cell outputs.
PiperOrigin-RevId: 269425711
Change-Id: Iacbe8444982ea462405379abf2c8a47666458d50
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Minor changes to little GAN colab including cell outputs.]


```

---

## 2019-09-16T13:28:20Z -- Use device name rather than LogicalDevice object in strategy ctor. (`79ae3540`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use device name rather than LogicalDevice object in strategy ctor.
PiperOrigin-RevId: 269320258
Change-Id: I3fb48f5e95162c6fc20de0a04ae8f0f280e6cfb8
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use device name rather than LogicalDevice object in strategy ctor.]


```

---

## 2019-09-16T11:48:25Z -- Merge pull request #145 from joaogui1:wrong-link (`9f81e8f5`)

**Author:** Sonnet Copybara <sonnetcopybara@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #145 from joaogui1:wrong-link
PiperOrigin-RevId: 269306806
Change-Id: I30ec0e488e80fe1872c73a2ba005dec8f5a172b7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 145 from joaogui1:wrong-link]


```

---

## 2019-09-16T09:41:24Z -- Merge pull request #144 from joaogui1:notebooks-version (`75c023b1`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #144 from joaogui1:notebooks-version
PiperOrigin-RevId: 268910984
Change-Id: If5550d3eccf8e7f37926b6c92cdd55859db5dd74
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 144 from joaogui1:notebooks-version]


```

---

## 2019-09-16T09:40:13Z -- Merge pull request #144 from joaogui1:notebooks-version (`1ddacbfe`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #144 from joaogui1:notebooks-version
PiperOrigin-RevId: 268908803
Change-Id: I1c43e8f05519c53b78e0c0b9a52b393f3fff7241
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 144 from joaogui1:notebooks-version]


```

---

## 2019-09-16T08:11:17Z -- Compress device strings in `snt.{format,log}_variables`. (`2a3a5d3c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Compress device strings in `snt.{format,log}_variables`.
```
>>> net = snt.nets.MLP([1000, 100, 10])
>>> net(tf.ones([1, 28 * 28]))
>>> print(utils.format_variables(net.variables))
Variable        Spec           Trainable  Device
========        ====           =========  ======
mlp/linear_0/w  f32[784,1000]  True       CPU
mlp/linear_0/b  f32[1000]      True       CPU
mlp/linear_1/w  f32[1000,100]  True       CPU
mlp/linear_1/b  f32[100]       True       CPU
mlp/linear_2/w  f32[100,10]    True       CPU
mlp/linear_2/b  f32[10]        True       CPU
```

PiperOrigin-RevId: 269275581
Change-Id: Ie62402659d3be399d94f4f4bb1867b2780184948
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Compress device strings in snt.formatlog_variables.]


```

---

## 2019-09-15T22:54:37Z -- Update notebook URLs. (`4176b52e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update notebook URLs.
PiperOrigin-RevId: 269215536
Change-Id: Ic51990d8d59591bfe0773efa7c40bf96ee0bc0f1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update notebook URLs.]


```

---

## 2019-09-15T16:12:28Z -- Temporarily disable flaky test on TPU. (`eae0bfcb`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Temporarily disable flaky test on TPU.
PiperOrigin-RevId: 269187867
Change-Id: I4f5db2ba06f5cfa2d7730c98930e00b39e73a341
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Temporarily disable flaky test on TPU.]


```

---

## 2019-09-13T15:35:37Z -- Internal change (`046bd973`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 268910789
Change-Id: If79aa9e6affeb5b72169e22ebd3af267e5aaaf43
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2019-09-13T14:37:13Z -- Add utils.smart_autograph decorator and associated tests for snt.Dropout. (`8db60b99`)

**Author:** Zafarali Ahmed <zafaraliahmed@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add utils.smart_autograph decorator and associated tests for snt.Dropout.
PiperOrigin-RevId: 268901938
Change-Id: Ic4b67e4d4abf406f3f4a4c05eb3607688ea1eb14
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add utils.smart_autograph decorator and associated tests for snt.Dropout.]


```

---

## 2019-09-13T08:48:15Z -- Add cross-replica batch norm to Sonnet (`33b9ffc8`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add cross-replica batch norm to Sonnet
PiperOrigin-RevId: 268860690
Change-Id: I33c22aa1d7ca368e611fe6847ff890e8636a8787
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add cross-replica batch norm to Sonnet]


```

---

## 2019-09-12T19:55:43Z -- Fixed typo (`6d8200f2`)

**Author:** joaogui1 <joaogui1@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed typo

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed typo]


```

---

## 2019-09-12T14:31:54Z -- Depend on TensorFlow 2.0.0rc1. (`4fa5ce6f`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Depend on TensorFlow 2.0.0rc1.
PiperOrigin-RevId: 268681106
Change-Id: I7e7ea4ada39fd61f6ba9edfcc78a0a537af912a3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Depend on TensorFlow 2.0.0rc1.]


```

---

## 2019-09-12T13:48:23Z -- Add optimizer base class to doc index. (`b2ab147e`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add optimizer base class to doc index.
PiperOrigin-RevId: 268674847
Change-Id: Ic31ccd72800a6aec721bf55fcdcbdd1bb5dd06cb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add optimizer base class to doc index.]


```

---

## 2019-09-12T00:41:29Z -- Fixed python version on mlp notebook (`b6b3af10`)

**Author:** joaogui1 <joaogui1@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed python version on mlp notebook

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed python version on mlp notebook]


```

---

## 2019-09-11T09:45:19Z -- Add dependency on TFP 0.8.0rc0. (`e887c945`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add dependency on TFP 0.8.0rc0.
PiperOrigin-RevId: 268418503
Change-Id: I41079f69ead7b8ead1b6c25f1a9cb2d2bba10136
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add dependency on TFP 0.8.0rc0.]


```

---

## 2019-09-10T16:04:27Z -- Temporarily pin gast 0.2.2 to work around autograph issue. (`92fc2de7`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Temporarily pin gast 0.2.2 to work around autograph issue.
Resolves #143

PiperOrigin-RevId: 268237268
Change-Id: Ia17139b063e32ab5448de850bac406d5de2cf13c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Temporarily pin gast 0.2.2 to work around autograph issue.]


```

---

## 2019-09-06T14:41:55Z -- Amend installation instructions for Sonnet 2. (`4e674ca2`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Amend installation instructions for Sonnet 2.
PiperOrigin-RevId: 267595045
Change-Id: I30fdc1b50626045b63ee506635ff65906046df02
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Amend installation instructions for Sonnet 2.]


```

---

## 2019-09-06T13:37:55Z -- Bump Sonnet 2 to beta. (`a750f28f`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump Sonnet 2 to beta.
PiperOrigin-RevId: 267586634
Change-Id: I737787c138141db3c3f5dee43ab617324270143f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump Sonnet 2 to beta.]


```

---

## 2019-09-05T16:27:48Z -- Tensorflow op that scales gradient for backwards pass. (`ecf4e351`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Tensorflow op that scales gradient for backwards pass.
PiperOrigin-RevId: 267388219
Change-Id: I9f55ff9c47de88653d9214563e433f2a27645acd
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Tensorflow op that scales gradient for backwards pass.]


```

---

## 2019-09-02T08:56:26Z -- Allow for minor differences in padded conv. (`eadee4d1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow for minor differences in padded conv.
PiperOrigin-RevId: 266738842
Change-Id: I8b0ec13778f8bae717212965de3e420f78d21f7e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow for minor differences in padded conv.]


```

---

## 2019-08-30T18:35:13Z -- Add leaky_clip_by_value function with custom_gradients to snt2/src/ops.py. (`89b89a81`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add leaky_clip_by_value function with custom_gradients to snt2/src/ops.py.
This function is tf.clip_by_value with a customized gradient: the gradient is set to zero when the input value is already out of valid range, and will be pushed away further by gradient-descent.

PiperOrigin-RevId: 266422427
Change-Id: If84b3550ca949f27974a55c8fd81abe0f2d02463
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add leaky_clip_by_value function with custom_gradients to snt2/src/ops.py.]


```

---

## 2019-08-28T09:19:53Z -- Add support for custom call functions in LayerAdapter. (`b30bee77`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add support for custom call functions in LayerAdapter.
PiperOrigin-RevId: 265866008
Change-Id: I1dc280d61ff68927fa0132a20bd220f0eccf0d86
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add support for custom call functions in LayerAdapter.]


```

---

## 2019-08-27T16:10:12Z -- Test Sonnet modules with the Keras symbolic API. (`7469ba61`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test Sonnet modules with the Keras symbolic API.
PiperOrigin-RevId: 265696801
Change-Id: I7e7a0879de372e93e0b406c80e79fd45e5c47242
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test Sonnet modules with the Keras symbolic API.]


```

---

## 2019-08-27T15:05:30Z -- Add parallel linear implementation (`c61b34ee`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add parallel linear implementation
PiperOrigin-RevId: 265684579
Change-Id: I601d48a90f48da2849f93027d3ca556e5f025107
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add parallel linear implementation]


```

---

## 2019-08-27T07:36:50Z -- Pin Sonnet to depend on TensorFlow 2.0.0rc0. (`fd894d93`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Pin Sonnet to depend on TensorFlow 2.0.0rc0.
PiperOrigin-RevId: 265626469
Change-Id: I8e44a0bd1cdd299be7933800def63054d9180afd
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Pin Sonnet to depend on TensorFlow 2.0.0rc0.]


```

---

## 2019-08-23T15:00:51Z -- Raise an exception if @snt.once decorated functions return values. (`adf38521`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Raise an exception if @snt.once decorated functions return values.
PiperOrigin-RevId: 265055616
Change-Id: I76240be6caae249186fd164ebcca2ecd5c3e9477
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Raise an exception if snt.once decorated functions return values.]


```

---

## 2019-08-22T16:44:40Z -- Add mixed_precision to the docs (`3a245991`)

**Author:** Loren Maggiore <lorenmaggiore@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add mixed_precision to the docs
PiperOrigin-RevId: 264855463
Change-Id: I3b3b98cd436a2e554f4a8b03cae0291be55f6392
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add mixed_precision to the docs]


```

---

## 2019-08-22T11:04:17Z -- Create and expose endpoint to disable mixed precision (`c10bc464`)

**Author:** Loren Maggiore <lorenmaggiore@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Create and expose endpoint to disable mixed precision
PiperOrigin-RevId: 264804278
Change-Id: If94ff7cf585dafbfd1485ecd25a7a8f491e4e2ae
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Create and expose endpoint to disable mixed precision]


```

---

## 2019-08-21T17:53:05Z -- Add Support for Mixed Precision Training (`886ddbaa`)

**Author:** Loren Maggiore <lorenmaggiore@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Support for Mixed Precision Training
Mixed precision speeds up training by casting
layers to lower precision formats when it is numerically
stable to do so. For example, this API can be used to cast
to float16 for performance increases when training on GPUs.

This is implemented by casting inputs and weights of layers
marked eligible for mixed precision, and scaling losses if
necessary to ensure that gradients do not underflow to zero.

PiperOrigin-RevId: 264644827
Change-Id: Id433bbadcafb0d2ebced0d2ffa88fb103ef37cf1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Support for Mixed Precision Training]


```

---

## 2019-08-21T15:58:21Z -- Bump TF nightly version. (`7ea9e5cf`)

**Author:** Loren Maggiore <lorenmaggiore@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump TF nightly version.
PiperOrigin-RevId: 264619411
Change-Id: I8923df3a8c9b91f6d87a4045ff48bef7be4d1670
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump TF nightly version.]


```

---

## 2019-08-19T10:12:24Z -- Add basic tests for Sonnet<>Keras compatibility. (`c8130014`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add basic tests for Sonnet<>Keras compatibility.
PiperOrigin-RevId: 264120153
Change-Id: I9599bc5cd886eff70a988e76c49a515d161380a7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add basic tests for SonnetKeras compatibility.]


```

---

## 2019-08-19T08:08:53Z -- Move descriptors out of function_test so they can be reused. (`7abdafd8`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move descriptors out of function_test so they can be reused.
PiperOrigin-RevId: 264104033
Change-Id: Ibfefa1bb8a9387f83b17e9c0ae451447439d47d3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move descriptors out of function_test so they can be reused.]


```

---

## 2019-08-15T22:33:05Z -- UnrolledLSTM now uses BlockLSTMV2 on CPU (`ef8f867a`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
UnrolledLSTM now uses BlockLSTMV2 on CPU
The key difference between BlockLSTM and BlockLSTMV2 is the gate layout,
the former uses ICFO (and thus requires permuting the weights/biases)
whereas the latter -- IFCO, same as Sonnet and CuDNN-RNN.

PiperOrigin-RevId: 263655721
Change-Id: I759c11bc835ec0be82fae8737197db9aaa634cfb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[UnrolledLSTM now uses BlockLSTMV2 on CPU]


```

---

## 2019-08-15T00:11:40Z -- Extended Reshape to support inputs with partially defined shapes (`2faf7acd`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Extended Reshape to support inputs with partially defined shapes
PiperOrigin-RevId: 263463224
Change-Id: Ie18ff94a61bad0d3cc95c97426269a0f6269ae61
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Extended Reshape to support inputs with partially defined shapes]


```

---

## 2019-08-14T06:56:25Z -- Changed reshape and flatten to call to Reshape/Flatten directly (`e95c0f6e`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Changed reshape and flatten to call to Reshape/Flatten directly
PiperOrigin-RevId: 263291221
Change-Id: I49039c833d807e50fd287d705df78ac07b99a317
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Changed reshape and flatten to call to Reshape/Flatten directly]


```

---

## 2019-08-14T06:19:07Z -- Slightly improved formatting in snt.Reshape and snt.Flatten docs (`06e49458`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Slightly improved formatting in snt.Reshape and snt.Flatten docs
PiperOrigin-RevId: 263288243
Change-Id: If284e2f666eb7669459f0fa74eff3fc3b1761a98
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Slightly improved formatting in snt.Reshape and snt.Flatten docs]


```

---

## 2019-08-13T13:20:54Z -- Update `tfds.load()` callers to specify `shuffle_files=True` when necessary. (`0f7c87e3`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update `tfds.load()` callers to specify `shuffle_files=True` when necessary.
PiperOrigin-RevId: 263123363
Change-Id: I339e9f85ecf0bb55c9be56bf08f326e5df27a8db
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update tfds.load callers to specify shuffle_filesTrue when necessary.]


```

---

## 2019-08-09T21:01:06Z -- Support additional {kw,}args in the first layer of Sequential. (`79434c16`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support additional {kw,}args in the first layer of Sequential.
PiperOrigin-RevId: 262628319
Change-Id: I1efa57e63babeefb42974b268d2ebf15b8a6a2a1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support additional kwargs in the first layer of Sequential.]


```

---

## 2019-08-08T12:58:29Z -- Raise an exception if the result of `{trainable_,}variables` is empty. (`4960be67`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Raise an exception if the result of `{trainable_,}variables` is empty.
>>> mod = snt.Linear(1)
>>> mod.variables
Traceback (most recent call last):
...
ValueError: ... pass an example input to the module.

This is a pretty common programming error so far, to mitigate we should default
to being strict and throwing an error if the result is empty. Users have an
escape hatch via `snt.allow_empty_variables` which can be used at an instance or
class level:

>>> @snt.allow_empty_variables
... class NeverHasVariables(snt.Module):
...   pass
...
>>> mod = NeverHasVariables()
>>> mod.variables
()

>>> mod = snt.Module()
>>> mod = snt.allow_empty_variables(mod)
>>> mod.variables
()

PiperOrigin-RevId: 262335275
Change-Id: Ica19adea95d64a50c0e6af8f6b2c71e81f30102d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Raise an exception if the result of trainable_variables is empty.]


```

---

## 2019-08-08T11:45:28Z -- Add new nets to the docs (`3dd59c8a`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add new nets to the docs
PiperOrigin-RevId: 262327233
Change-Id: I0ebe6f0d13f6d78e7679326d2540d1df36ada6e7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add new nets to the docs]


```

---

## 2019-08-08T10:48:42Z -- Explicitly compare variables by id in BBB. (`a5fe4a96`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Explicitly compare variables by id in BBB.
PiperOrigin-RevId: 262321332
Change-Id: I5d0608c298461f3c695ad9aa7682410c11e23bfd
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Explicitly compare variables by id in BBB.]


```

---

## 2019-08-08T10:25:11Z -- Add distributed CIFAR-10 example. (`385231d5`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add distributed CIFAR-10 example.
PiperOrigin-RevId: 262318776
Change-Id: Ie36d61e82062f9d579f205fffd0cf6474ec9e03c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add distributed CIFAR-10 example.]


```

---

## 2019-08-07T09:09:35Z -- Temporarily disable msan due to legit memory leak. (`7dc7ae94`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Temporarily disable msan due to legit memory leak.
PiperOrigin-RevId: 262091579
Change-Id: I969e94d5507916d1a7d30fc825f623cbde51eca6
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Temporarily disable msan due to legit memory leak.]


```

---

## 2019-08-07T07:51:12Z -- Add batch functions to docs (`f0d39751`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add batch functions to docs
PiperOrigin-RevId: 262080945
Change-Id: I6494d1c13c2919c748bcccefbfaf31623c052378
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add batch functions to docs]


```

---

## 2019-08-06T15:54:38Z -- Add metrics to docs (`391b82ad`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add metrics to docs
PiperOrigin-RevId: 261923432
Change-Id: I04f74974767a6568bcdc5dcf1dd1f322bc733f8b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add metrics to docs]


```

---

## 2019-08-05T12:44:37Z -- Increase test timeout for conformance tests. (`1375c9fc`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Increase test timeout for conformance tests.
PiperOrigin-RevId: 261667670
Change-Id: Ia54d44e255479bc15abab6cb32bc75f2a062cbf4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Increase test timeout for conformance tests.]


```

---

## 2019-08-05T09:22:26Z -- Disable ResNet v2 for now due to regression in performance (`ccc3d1b9`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Disable ResNet v2 for now due to regression in performance
Also provide default bn_args and expose the base ResNet

PiperOrigin-RevId: 261644971
Change-Id: I91db8401722ad0d2ae9179665533d2c1116ae290
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Disable ResNet v2 for now due to regression in performance]


```

---

## 2019-08-01T16:25:07Z -- Fixed a bug in UnrollTest.testVariableLengthRange (`c6895efb`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed a bug in UnrollTest.testVariableLengthRange
Tensor equality currently compares based on type/id and not the contents
of a tensor.

PiperOrigin-RevId: 261135416
Change-Id: I969d0a39aeaddc2b520bdca43d7b4142f0ca4927
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed a bug in UnrollTest.testVariableLengthRange]


```

---

## 2019-07-29T12:07:19Z -- Ensure distribute_tests check intended behavior (`dde56507`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure distribute_tests check intended behavior
Previously the random seed was being set in all tests meaning that all replicas had the same result due to have the same random seed not due to being created with the same values

PiperOrigin-RevId: 260476569
Change-Id: Ib0e0dfc0330a737ac330b2f258fb640d78a7d844
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure distribute_tests check intended behavior]


```

---

## 2019-07-26T11:22:43Z -- Added DNC util library. (`3534f5c8`)

**Author:** Malcolm Reynolds <malcolmreynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added DNC util library.
PiperOrigin-RevId: 260121784
Change-Id: Ibd9a1dca0fbc11d2388f27f78d2a293872836ab0
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added DNC util library.]


```

---

## 2019-07-25T15:48:05Z -- Add Sonnet 2 "Little GAN" notebook (`61ca530f`)

**Author:** Jeff Donahue <jeffdonahue@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Sonnet 2 "Little GAN" notebook
PiperOrigin-RevId: 259953991
Change-Id: I7375e59a3bf92dff56e088c31331588bcbfdd591
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Sonnet 2 Little GAN notebook]


```

---

## 2019-07-25T14:02:21Z -- Export ResNet50 as snt.nets.ResNet50 (`eb9729f7`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Export ResNet50 as snt.nets.ResNet50
Note that the conformance tests are performed using ResNet which isn't exported but will perform the same for efficiency purposes

PiperOrigin-RevId: 259939223
Change-Id: I2b66bfd496847670d97d73700a245aa55afa66df
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Export ResNet50 as snt.nets.ResNet50]


```

---

## 2019-07-25T13:36:20Z -- Internal docutils now supports Python 3 :) (`ad1c2370`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal docutils now supports Python 3 :)
PiperOrigin-RevId: 259936052
Change-Id: I846bb845dcaf6c6c488a8d694f07951bbe80749a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal docutils now supports Python 3 :]


```

---

## 2019-07-25T10:36:35Z -- Added a CPU specialization for UnrolledLSTM (`34fa863b`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added a CPU specialization for UnrolledLSTM
PiperOrigin-RevId: 259917731
Change-Id: I06e17354a0e14b29688a15b74b13033d6c2d4a49
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added a CPU specialization for UnrolledLSTM]


```

---

## 2019-07-25T08:56:33Z -- Bump TF nightly version. (`de4cb89f`)

**Author:** Jeff Donahue <jeffdonahue@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump TF nightly version.
PiperOrigin-RevId: 259905862
Change-Id: Iac34ef9704740950be3b73bd2a2730fcc8101db7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump TF nightly version.]


```

---

## 2019-07-25T07:28:41Z -- Change _specialize_per_device to special-case eager mode (`9dc3cf79`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change _specialize_per_device to special-case eager mode
In eager mode a specialization is chosen based on the current device
scope, whereas in graph (tf.function) that choice is delegated to the
implementation selector pass in Grappler.

Handling the two cases differently is needed as currently the implementation
selector seems to itgnore tf.device(...) context outside of tf.function,
i.e.

with tf.device("GPU:0"):
_specialized_fn(...)

will not be executed on "GPU:0", whereas

def _specialized_fn(...):
with tf.device("GPU:0"):
...

_specialized_fn(...)

will be.

PiperOrigin-RevId: 259895315
Change-Id: I4a1e940f4d84235db4695691347ec31b37a0897e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change _specialize_per_device to special-case eager mode]


```

---

## 2019-07-24T16:05:14Z -- Add ResNet model to nets (`7d253078`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add ResNet model to nets
This has been trained to > 76.2% accuracy on both TPU and GPU and roughly matches the model defined here https://github.com/tensorflow/tpu/tree/master/models/official/resnet

PiperOrigin-RevId: 259751749
Change-Id: Ice868c2a97f643e854768657898bb6970f7e559b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add ResNet model to nets]


```

---

## 2019-07-24T11:36:10Z -- Porting DNC to sonnet 2: read, control and write modules done. (`e668c86c`)

**Author:** Malcolm Reynolds <malcolmreynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Porting DNC to sonnet 2: read, control and write modules done.
PiperOrigin-RevId: 259716333
Change-Id: I0337526b1eb3439efe4fdb250f166e67f2094f55
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Porting DNC to sonnet 2: read control and write modules done.]


```

---

## 2019-07-23T13:26:38Z -- Enabled UnrolledLSTM test on TPU (`02804fb5`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Enabled UnrolledLSTM test on TPU
The linked bug is no longer relevant because AutoGraph special-cases loops
over tf.range.

PiperOrigin-RevId: 259524221
Change-Id: I1ae4ae619552b63260b5677116977f7daf330a78
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Enabled UnrolledLSTM test on TPU]


```

---

## 2019-07-23T10:56:46Z -- Export Optimizer base class. (`bdf44644`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Export Optimizer base class.
PiperOrigin-RevId: 259507614
Change-Id: I7c6486bc975575fa67723c4a7f6052b44463cda3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Export Optimizer base class.]


```

---

## 2019-07-23T10:25:31Z -- _noinline is not longer needed in _specialize_per_device (`5f06ce05`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
_noinline is not longer needed in _specialize_per_device
The corresponding Grappler bug has been fixed.

PiperOrigin-RevId: 259504851
Change-Id: I1f46229aed69a10d0c9d6c743a550359639fc3ad
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[_noinline is not longer needed in _specialize_per_device]


```

---

## 2019-07-23T09:02:52Z -- Use Mirrored Variables on TPU due to bug in SyncOnRead Variables causing model divergence (`30e9a817`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use Mirrored Variables on TPU due to bug in SyncOnRead Variables causing model divergence
PiperOrigin-RevId: 259494672
Change-Id: I2dc6dc0b357c6831ab935a25724d44c9c35fa3a6
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use Mirrored Variables on TPU due to bug in SyncOnRead Variables causing model divergence]


```

---

## 2019-07-22T14:17:06Z -- Small fix to docstring. (`fa5e406e`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Small fix to docstring.
PiperOrigin-RevId: 259321675
Change-Id: I586dd277526d9d7e2989c6a75f5b9d4a3930924c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Small fix to docstring.]


```

---

## 2019-07-17T17:36:44Z -- Remove TODO for casting updates to correct dtype. (`0aad7808`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove TODO for casting updates to correct dtype.
This cast would potentially be very expensive. It's better that the user gets an error rather than a silent performance regression.

PiperOrigin-RevId: 258596215
Change-Id: I0bca1a92a124aa767bbac21649e345c3f1ca53f1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove TODO for casting updates to correct dtype.]


```

---

## 2019-07-17T15:35:18Z -- Check for a compatible distribution strategy on every call to apply. (`0bfca893`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Check for a compatible distribution strategy on every call to apply.
PiperOrigin-RevId: 258574659
Change-Id: If4f255932e79cc71b559472fdb8792c488d433b6
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Check for a compatible distribution strategy on every call to apply.]


```

---

## 2019-07-17T14:27:04Z -- Remove optimizer_utils_test.py. These tests are redundant. (`bc63faf9`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove optimizer_utils_test.py. These tests are redundant.
PiperOrigin-RevId: 258564173
Change-Id: I7686ec69836bf2d5f874016af584cbcad2d91f52
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove optimizer_utils_test.py. These tests are redundant.]


```

---

## 2019-07-17T11:21:12Z -- Expose custom_variable_getter in the Sonnet API. (`e5eb5663`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose custom_variable_getter in the Sonnet API.
PiperOrigin-RevId: 258542219
Change-Id: I736bb68b9897a3a14a4e5cf31d39c92bcf9f1025
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose custom_variable_getter in the Sonnet API.]


```

---

## 2019-07-17T10:41:28Z -- Bump TF nightly version. (`7e1a7769`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump TF nightly version.
PiperOrigin-RevId: 258537954
Change-Id: I5f45b4afcb0b23914d9a1c422a4b941a147be8f9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump TF nightly version.]


```

---

## 2019-07-16T20:34:48Z -- Strip base_dtype from Sonnet 2. (`857d877b`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Strip base_dtype from Sonnet 2.
base_dtype only differentiates between reference and non-reference dtypes. In
TensorFlow 2 there is no way to produce a reference dtype.

PiperOrigin-RevId: 258430193
Change-Id: Ib4e2a2ab9c131f8f39e8750d077c8a2f78ffde4f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Strip base_dtype from Sonnet 2.]


```

---

## 2019-07-16T16:41:11Z -- Move common optimizer tests into a base class. (`a05610e8`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move common optimizer tests into a base class.
PiperOrigin-RevId: 258384006
Change-Id: I15fe639054586b86b0e2eaa284f63a2a4fe3ccec
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move common optimizer tests into a base class.]


```

---

## 2019-07-16T16:10:10Z -- Add distribution strategy check to SGD. (`6775fdd5`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add distribution strategy check to SGD.
PiperOrigin-RevId: 258378736
Change-Id: I5856f7b628523e4741a19ee8f246daaabcbf2104
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add distribution strategy check to SGD.]


```

---

## 2019-07-15T15:17:54Z -- Fix `BatchNorm` constructor. (`da28ab54`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix `BatchNorm` constructor.
PiperOrigin-RevId: 258165916
Change-Id: I580242b1d9baf3296def9deae43f715e183b043e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix BatchNorm constructor.]


```

---

## 2019-07-15T14:29:19Z -- Change module-internal constant name to match style guide. (`ec80fb3f`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change module-internal constant name to match style guide.
PiperOrigin-RevId: 258157624
Change-Id: Ib9bf88a3ac77c653548ad86f57049ce48415d9a8
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change module-internal constant name to match style guide.]


```

---

## 2019-07-15T14:09:56Z -- Raise an error if all updates given to optimizer.apply are None. (`3d26748e`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Raise an error if all updates given to optimizer.apply are None.
PiperOrigin-RevId: 258155179
Change-Id: I4b6885a07824eb42da3b81d57557b39f693bba06
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Raise an error if all updates given to optimizer.apply are None.]


```

---

## 2019-07-15T13:43:08Z -- Port `log_variables` and `format_variables` from Sonnet v1 to v2. (`86405e81`)

**Author:** Jeff Donahue <jeffdonahue@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Port `log_variables` and `format_variables` from Sonnet v1 to v2.
Changes from v1 to v2:

- `variables` argument to `log_variables` can't be None (must be an explicit iterable as there are no global/local collections)
- Removed collections (collections gone in TF2)
- Added `Trainable` column (replacing collections)
- Used `var.name.split(":")[0]` instead of `var.op.name` to get name without the `:0` suffix (var.op doesn't work in TF2)
- No more resource/legacy distinction displayed (all vars are resources in TF2)

PiperOrigin-RevId: 258151411
Change-Id: I5ad75b1bcc00cb77f8d7a1822dff1a1daabb513f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Port log_variables and format_variables from Sonnet v1 to v2.]


```

---

## 2019-07-15T10:06:52Z -- Remove TODO for caching casts. These are all scalars so the casts cost little. (`61a12193`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove TODO for caching casts. These are all scalars so the casts cost little.
PiperOrigin-RevId: 258126205
Change-Id: I747d01781182d22ff06774a4513c542c41ebefd1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove TODO for caching casts. These are all scalars so the casts cost little.]


```

---

## 2019-07-15T09:02:53Z -- Change the eps to 1e-5 within the remaining normalization modules (`25846946`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change the eps to 1e-5 within the remaining normalization modules
PiperOrigin-RevId: 258117759
Change-Id: I98a9c4b076fda181b5f53d5853bf850b02bf64ce
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change the eps to 1e-5 within the remaining normalization modules]


```

---

## 2019-07-12T15:04:57Z -- Add support for IndexedSlices to Adam. (`57dea91f`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add support for IndexedSlices to Adam.
PiperOrigin-RevId: 257803926
Change-Id: I63fd9fe9ccee0f4f4084e627d09f816191dde161
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add support for IndexedSlices to Adam.]


```

---

## 2019-07-12T14:36:46Z -- Bump TF nightly version. (`8283feb0`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump TF nightly version.
PiperOrigin-RevId: 257800140
Change-Id: I5f7b401b76a84816e38dfab64c28da11ec97ab85
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump TF nightly version.]


```

---

## 2019-07-10T17:44:35Z -- Correct docstring for the Adam update rule. (`f1d02182`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Correct docstring for the Adam update rule.
The implementation is correct, but the docstring is wrong. The beta2 debiasing was applied to sqrt(v) + epsilon making epsilon time dependent. While it should only apply the debiasing to v or sqrt(v).

PiperOrigin-RevId: 257435546
Change-Id: I309f243522a54884fba8fb4d08a904b5c4ebcd29
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Correct docstring for the Adam update rule.]


```

---

## 2019-07-10T17:05:13Z -- Fix instance norm to normalize over the spatial dimensions only instead of channels (`9cdfc977`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix instance norm to normalize over the spatial dimensions only instead of channels
Also change the default eps to 1e-5

PiperOrigin-RevId: 257426749
Change-Id: Ie849121679fcf1d540f29be677a792cdff64e845
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix instance norm to normalize over the spatial dimensions only instead of channels]


```

---

## 2019-07-09T11:12:44Z -- Tidy-up Adam to more closely follow the published version. (`de68259f`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Tidy-up Adam to more closely follow the published version.
PiperOrigin-RevId: 257163944
Change-Id: If6003343e284cfe72fad44bdebb8794fdad3d1af
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Tidy-up Adam to more closely follow the published version.]


```

---

## 2019-07-08T16:49:37Z -- Test optimizers with @tf.function. (`49859b56`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test optimizers with @tf.function.
PiperOrigin-RevId: 256998273
Change-Id: I046e7f0eb252c2490c28859fd1756c1322601365
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test optimizers with tf.function.]


```

---

## 2019-07-08T13:54:05Z -- Tidy SGD tests. (`2ddd050f`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Tidy SGD tests.
PiperOrigin-RevId: 256969054
Change-Id: Ib9a63c58f8f8ee5c6ac27b811559cf34d2b2e2e2
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Tidy SGD tests.]


```

---

## 2019-07-08T09:47:27Z -- Test all Sonnet modules with @tf.function and dynamic batch size. (`28453b37`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test all Sonnet modules with @tf.function and dynamic batch size.
PiperOrigin-RevId: 256938014
Change-Id: I855c7e986a36d53a7128c3c64f3f8d0e648a2487
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test all Sonnet modules with tf.function and dynamic batch size.]


```

---

## 2019-07-08T09:03:14Z -- Use `.shape` instead of `.get_shape()` consistently. (`bd030b15`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use `.shape` instead of `.get_shape()` consistently.
PiperOrigin-RevId: 256932610
Change-Id: I782334eb68a318ce498601e3e162eed5bc07166d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use .shape instead of .get_shape consistently.]


```

---

## 2019-07-08T06:15:36Z -- Support dynamic shapes in BatchApply. (`e164c357`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support dynamic shapes in BatchApply.
PiperOrigin-RevId: 256912587
Change-Id: Ic5d92b3704782bc4068a6a5249bca6627598695c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support dynamic shapes in BatchApply.]


```

---

## 2019-07-05T20:51:30Z -- Camel to snake replicatorOrSkip. (`ccba0f90`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Camel to snake replicatorOrSkip.
PiperOrigin-RevId: 256700843
Change-Id: I7648c0cf0192eaae07b9d4955fc121de727ff9f5
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Camel to snake replicatorOrSkip.]


```

---

## 2019-07-05T16:51:01Z -- Add support for IndexedSlices to RMSProp. (`0c1aa77f`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add support for IndexedSlices to RMSProp.
PiperOrigin-RevId: 256684090
Change-Id: I53a23bf22b559032b95e0584b9e30e66d21c1905
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add support for IndexedSlices to RMSProp.]


```

---

## 2019-07-05T15:23:08Z -- Test all strategies in checkpoint test. (`c65f26a3`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test all strategies in checkpoint test.
PiperOrigin-RevId: 256676447
Change-Id: I7f51266f1b1cb550f642b2866ad4538e5ac8a1e7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test all strategies in checkpoint test.]


```

---

## 2019-07-05T13:18:47Z -- Remove use_dropout from `snt.nets.MLP`. (`63a3d261`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove use_dropout from `snt.nets.MLP`.
PiperOrigin-RevId: 256664562
Change-Id: I64d859a63d70ff59b6a5100d54c1fffa0882bbf0
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove use_dropout from snt.nets.MLP.]


```

---

## 2019-07-05T10:52:19Z -- Fix Momentum when using tf.function. (`da4f2a9e`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix Momentum when using tf.function.
scatter_nd_sub behaves the same as scatter_sub for IndexedSlices in Eager mode but not in Graph mode. We can just use scatter_sub though.

PiperOrigin-RevId: 256650570
Change-Id: Ic7c1493bb827c90f54abc54152b690779069a99e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix Momentum when using tf.function.]


```

---

## 2019-07-05T10:23:51Z -- Make dropout_rate required when use_dropout=True. (`e900c4a5`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make dropout_rate required when use_dropout=True.
PiperOrigin-RevId: 256648247
Change-Id: I9cdc5ae6b439cccb8d4a6278e17af70d5d16fad5
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make dropout_rate required when use_dropoutTrue.]


```

---

## 2019-07-05T08:58:51Z -- Re-enable linear tests on GPU. (`f0740b2a`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Re-enable linear tests on GPU.
PiperOrigin-RevId: 256636833
Change-Id: I219add98356985b8fd037855a9914ef00b3cf8a2
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Re-enable linear tests on GPU.]


```

---

## 2019-07-04T17:53:58Z -- Fix SGD when using tf.function. (`3fd97a0b`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix SGD when using tf.function.
scatter_nd_sub behaves the same as scatter_sub for IndexedSlices in Eager mode but not in Graph mode. We can just use scatter_sub though.

PiperOrigin-RevId: 256568297
Change-Id: Ie8267e0f09d26cf92e8256f821423f170e63493a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix SGD when using tf.function.]


```

---

## 2019-07-04T14:35:16Z -- Add optional dropout to sonnet's MLP class. (`3647f96b`)

**Author:** Siddhant Jayakumar <siddhantjayakumar@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add optional dropout to sonnet's MLP class.
PiperOrigin-RevId: 256550053
Change-Id: Ie6f3df412ac1cd448c16d3d4476db9d6e915fc34
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add optional dropout to sonnets MLP class.]


```

---

## 2019-07-04T13:43:21Z -- Allow BatchApply to work with non-Tensor kwargs. (`d2d22e79`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow BatchApply to work with non-Tensor kwargs.
PiperOrigin-RevId: 256544689
Change-Id: Icb6d2d775852099d00197d0bc81c9ca84716593d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow BatchApply to work with non-Tensor kwargs.]


```

---

## 2019-07-03T16:55:05Z -- Add Sonnet v2 `Sum` and `Mean` metrics. (`98945bd9`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Sonnet v2 `Sum` and `Mean` metrics.
PiperOrigin-RevId: 256387058
Change-Id: Idba40080c927da761ac4a3bdd24e5333732d771a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Sonnet v2 Sum and Mean metrics.]


```

---

## 2019-07-03T13:06:20Z -- Add support for IndexedSlices to Momentum. (`4671afa5`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add support for IndexedSlices to Momentum.
PiperOrigin-RevId: 256352857
Change-Id: I626c88afd803a7fbed7c090a1eb7d0e8a3be4304
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add support for IndexedSlices to Momentum.]


```

---

## 2019-07-03T10:43:19Z -- Run Replicator and TpuReplicator tests on TPU. (`8f9cef1f`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run Replicator and TpuReplicator tests on TPU.
PiperOrigin-RevId: 256337202
Change-Id: I1afee0aaf1e0b5f59a8a11f96f6283f569ff4d30
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run Replicator and TpuReplicator tests on TPU.]


```

---

## 2019-07-02T16:30:57Z -- Add functional variants of snt.{Flatten,Reshape}. (`9a9f6982`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add functional variants of snt.{Flatten,Reshape}.
For inline use this is more convenient:

snt.flatten(x, ...)  # vs. `snt.Flatten(..)(x)`

PiperOrigin-RevId: 256179961
Change-Id: Ifffd0fa101abaea30063ca0777f2a1fd643fcd70
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add functional variants of snt.FlattenReshape.]


```

---

## 2019-07-02T12:15:41Z -- Improve optimizer docstrings. (`fbc59cb8`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve optimizer docstrings.
PiperOrigin-RevId: 256145048
Change-Id: Ie775d9ad079c159dfc12147771a7dc06eb007684
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve optimizer docstrings.]


```

---

## 2019-07-02T06:38:29Z -- Increase tollerance for XLA tests. (`41d1214e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Increase tollerance for XLA tests.
PiperOrigin-RevId: 256105373
Change-Id: I05eb8b42ef1e18a366d829878d51213a99dd816e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Increase tollerance for XLA tests.]


```

---

## 2019-07-01T21:09:49Z -- Skip over testInitialization in recurrent_test.py. (`d44f237e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Skip over testInitialization in recurrent_test.py.
PiperOrigin-RevId: 256029756
Change-Id: I7c2727c6143196216ef0df946de3ed12150d26e4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Skip over testInitialization in recurrent_test.py.]


```

---

## 2019-06-30T18:48:27Z -- Set `module.__test__` for bazel doctest. (`7f8d128d`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Set `module.__test__` for bazel doctest.
We run two lots of doctest, first via bazel to provide quick feedback when
testing iteratively. Secondly we run doctest on all the generated Sphinx docs
via `test.sh` to ensure samples in our documentation run as described.

When running via bazel we need to set `__test__` appropriately so doctest runs
over imported symbols. This is not needed for the Sphinx tests since Sphinx runs
all code samples that are included in the docs.

PiperOrigin-RevId: 255849190
Change-Id: I697c248fa1c198b7fae201e7d12a3572a7a55d82
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Set module.__test__ for bazel doctest.]


```

---

## 2019-06-29T19:32:49Z -- Simplify checkpoint example. (`27f85630`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Simplify checkpoint example.
PiperOrigin-RevId: 255769571
Change-Id: Ia9c9e5a5dae6614ac1065c4a9d400fce9eb296fb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Simplify checkpoint example.]


```

---

## 2019-06-28T23:23:11Z -- Update documentation for {Tpu,}Replicator. (`5317852e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update documentation for {Tpu,}Replicator.
PiperOrigin-RevId: 255688603
Change-Id: Ifa1e34f5647310d1cf2b1c686ec01831fe94f9aa
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update documentation for TpuReplicator.]


```

---

## 2019-06-28T22:25:37Z -- Add TpuReplicator to docs. (`3f9568d0`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add TpuReplicator to docs.
PiperOrigin-RevId: 255679152
Change-Id: Ie2b5552dd0b6966aa340660aa45939b6c518152a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add TpuReplicator to docs.]


```

---

## 2019-06-28T14:25:10Z -- Add support for IndexedSlices to SGD. (`892bdaa5`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add support for IndexedSlices to SGD.
PiperOrigin-RevId: 255596723
Change-Id: I3aeb4a83b6bb457bd33c8250ff2c4f07412114ae
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add support for IndexedSlices to SGD.]


```

---

## 2019-06-28T13:06:10Z -- Add tests for optimizer utils. (`b4a00d03`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add tests for optimizer utils.
PiperOrigin-RevId: 255588269
Change-Id: I26f64d3c5049dfd77f2da834e97f30adab45e60a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add tests for optimizer utils.]


```

---

## 2019-06-28T11:43:02Z -- Rename strategy to replicator. (`b4ea38f5`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Rename strategy to replicator.
PiperOrigin-RevId: 255580200
Change-Id: I0dfa2e2787b6470831800df4a344222cd3c02f3c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Rename strategy to replicator.]


```

---

## 2019-06-28T10:28:28Z -- Tiny fix to MNIST example. (`dbd585cc`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Tiny fix to MNIST example.
PiperOrigin-RevId: 255573628
Change-Id: Ib4383315d629cbe236623924ffb93698fc770d22
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Tiny fix to MNIST example.]


```

---

## 2019-06-26T14:42:49Z -- Add TpuReplicator. (`a64dd044`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add TpuReplicator.
PiperOrigin-RevId: 255185815
Change-Id: I5135fd82c5d65043a85bc0af054652e5de0c8376
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add TpuReplicator.]


```

---

## 2019-06-26T10:46:07Z -- Expose Exponential Moving Average as part of Sonnet and add to Goldens (`d723d03c`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose Exponential Moving Average as part of Sonnet and add to Goldens
Also add tests for batch norm which update the moving statistics. This required minor changes to the conformance tests to allow the testing of non pure layers

PiperOrigin-RevId: 255156882
Change-Id: Ibf8d1dc7498a3bed5134120ec9dca3391b6af01b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose Exponential Moving Average as part of Sonnet and add to Goldens]


```

---

## 2019-06-26T10:38:53Z -- Bump TF nightlties. (`c11c6821`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump TF nightlties.
PiperOrigin-RevId: 255156136
Change-Id: Id0e740b47f7f08ad1def46d9dd0950ced150bec4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump TF nightlties.]


```

---

## 2019-06-25T10:52:26Z -- Bump TF nightlties. (`498caa4e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump TF nightlties.
PiperOrigin-RevId: 254939616
Change-Id: I1377f3cac4fc05b8a20c8608f6990ceaf17c520b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump TF nightlties.]


```

---

## 2019-06-24T17:01:08Z -- Docstring changes for initializers (`566e74c1`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Docstring changes for initializers
PiperOrigin-RevId: 254778967
Change-Id: Ia7028ecb5cc4c42aef2373d31f77fb044dfd19c5
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Docstring changes for initializers]


```

---

## 2019-06-24T14:32:47Z -- Docstring fixes for GroupNorm (`578b65bc`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Docstring fixes for GroupNorm
PiperOrigin-RevId: 254754178
Change-Id: I18732af6cb7648dcf2ef2e1bd0bda3eec374d979
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Docstring fixes for GroupNorm]


```

---

## 2019-06-23T13:23:52Z -- \nInternal refactor\n (`aaa61637`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
\nInternal refactor\n
PiperOrigin-RevId: 254632879
Change-Id: I724c50476b232db7f967a75a04e2fb6ea1debb86
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[nInternal refactorn]


```

---

## 2019-06-21T15:57:33Z -- Switch Reference and Fast optimizers. (`6436259c`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Switch Reference and Fast optimizers.
PiperOrigin-RevId: 254406198
Change-Id: Idcab0a287ea496002c58c6b1e2bdeb56e3a7f1c4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Switch Reference and Fast optimizers.]


```

---

## 2019-06-20T15:49:06Z -- Improved docstring formatting for normalization. (`19eb6663`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improved docstring formatting for normalization.
PiperOrigin-RevId: 254205449
Change-Id: Icf4e4e3cbcc09650128857fbfdd0ac322449e309
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improved docstring formatting for normalization.]


```

---

## 2019-06-20T15:48:20Z -- Docstring gardening in recurrent. (`aa57dfaa`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Docstring gardening in recurrent.
PiperOrigin-RevId: 254205306
Change-Id: I6f974905e568f14d168e93ad20850c1e1931b0e3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Docstring gardening in recurrent.]


```

---

## 2019-06-20T13:35:22Z -- Support linking to methods of exported TF classes. (`0b1a2f03`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support linking to methods of exported TF classes.
PiperOrigin-RevId: 254185428
Change-Id: Ie71e9d1dd54c2c36cf555b186db56e54d300ce97
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support linking to methods of exported TF classes.]


```

---

## 2019-06-20T12:42:53Z -- Add smart autograph decorator to ensure EMA and Batch norm work with autograph=False (`bdf44ae4`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add smart autograph decorator to ensure EMA and Batch norm work with autograph=False
PiperOrigin-RevId: 254179404
Change-Id: Ia5b137ebdff8707751231528ee6b7dbfd74cbb24
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add smart autograph decorator to ensure EMA and Batch norm work with autographFalse]


```

---

## 2019-06-20T11:04:56Z -- Make BatchApply visible in the external API. (`d0df4c1c`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make BatchApply visible in the external API.
PiperOrigin-RevId: 254169987
Change-Id: If2a9a5462d2b14243e7f741b92a3b17cc3906554
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make BatchApply visible in the external API.]


```

---

## 2019-06-18T14:09:47Z -- Add batch apply module. (`2db82da1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add batch apply module.
PiperOrigin-RevId: 253783307
Change-Id: I03a98d11fccc2424563d538774179a0699432ac4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add batch apply module.]


```

---

## 2019-06-17T20:50:09Z -- Run generated docs through doctest. (`eb911887`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run generated docs through doctest.
PiperOrigin-RevId: 253653578
Change-Id: I9f8c2e9164814ab4fb1b2e7ad54262f016217b63
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run generated docs through doctest.]


```

---

## 2019-06-17T20:49:40Z -- Docstring gardening for base.py. (`e51d47ea`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Docstring gardening for base.py.
PiperOrigin-RevId: 253653478
Change-Id: I1c590907b821d0474654200d20eb3834589d7f29
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Docstring gardening for base.py.]


```

---

## 2019-06-17T20:15:22Z -- Initial docs on modules. (`06483b26`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Initial docs on modules.
PiperOrigin-RevId: 253646712
Change-Id: I9bfa5f5d63931f36c13b402f899671fc73c41b44
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Initial docs on modules.]


```

---

## 2019-06-17T16:46:18Z -- Add UnrolledLSTM. (`c911af3b`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add UnrolledLSTM.
UnrolledLSTM is an efficient version of

snt.dynamic_unroll(snt.LSTM(...), ...)

which is specialized per-device. Currently, the only available specialization
is CuDNN-RNN, however, an efficient CPU from tf.contrib.rnn is on the way.

PiperOrigin-RevId: 253599338
Change-Id: I8faaa460657cafb199baae24bee97dfc9614748a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add UnrolledLSTM.]


```

---

## 2019-06-17T09:10:07Z -- Test Sonnet 2 with XLA {compile,jit_scope}. (`154a5522`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Test Sonnet 2 with XLA {compile,jit_scope}.
PiperOrigin-RevId: 253539038
Change-Id: If6287b887df447a609959ae2a3b3c0ffdc3b0adc
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Test Sonnet 2 with XLA compilejit_scope.]


```

---

## 2019-06-16T21:50:07Z -- Cross link examples/notebooks for easier discovery. (`73702b77`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Cross link examples/notebooks for easier discovery.
PiperOrigin-RevId: 253485297
Change-Id: I17b4e27407dd2f4731e11371deeb6ee8f543188d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Cross link examples/notebooks for easier discovery.]


```

---

## 2019-06-14T10:27:36Z -- Use six.moves to reference `reload`. (`ff19efc1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use six.moves to reference `reload`.
PiperOrigin-RevId: 253199825
Change-Id: Ifb9bf182572900a813ea1b0dbbda60f82495eac1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use six.moves to reference reload.]


```

---

## 2019-06-13T20:29:07Z -- Support `snt = importlib.reload(snt)`. (`d5952ff5`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support `snt = importlib.reload(snt)`.
PiperOrigin-RevId: 253093790
Change-Id: I58187c7be651649cb07bc4209179d987f28f71f7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support snt  importlib.reloadsnt.]


```

---

## 2019-06-13T15:23:53Z -- Change Batch Norm such that the moving mean and variance are the same for Fused and none fused ops. (`b19c28c2`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change Batch Norm such that the moving mean and variance are the same for Fused and none fused ops.
This allows for more flexibility later to chose which is the most efficient op to use. Also includes changes to ensure that all variables are created on the first call which is necessary to correctly restore checkpoints

PiperOrigin-RevId: 253031884
Change-Id: Ic9fd9f9b9f5e1686df2fd4643a9b84757ab0a795
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change Batch Norm such that the moving mean and variance are the same for Fused and none fused ops.]


```

---

## 2019-06-13T15:01:33Z -- Add initial custom getters support. (`d806d593`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add initial custom getters support.
PiperOrigin-RevId: 253028381
Change-Id: Ie433308da1424da608a516e15f9bb5d57c869969
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add initial custom getters support.]


```

---

## 2019-06-13T09:48:32Z -- Resolve symbols beyond the top level of TF. (`3429a527`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Resolve symbols beyond the top level of TF.
PiperOrigin-RevId: 252992905
Change-Id: Id7b251e533c48becc2eb1fe7ae6017af1e5951a4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Resolve symbols beyond the top level of TF.]


```

---

## 2019-06-13T09:17:14Z -- Make tf.name_scope(..) reentrant in TF2. (`5c76b91c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make tf.name_scope(..) reentrant in TF2.
PiperOrigin-RevId: 252989408
Change-Id: I97c6610da724ac6d9829d42676b95554f90dda67
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make tf.name_scope.. reentrant in TF2.]


```

---

## 2019-06-12T10:52:47Z -- Remove workaround for lack of trainable ReplicaLocalVariables. (`b990ccc0`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove workaround for lack of trainable ReplicaLocalVariables.
PiperOrigin-RevId: 252795306
Change-Id: I8dfb0e3239fd4f6f1b3f21f20ccac4d796ac8aed
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove workaround for lack of trainable ReplicaLocalVariables.]


```

---

## 2019-06-11T20:35:16Z -- Add variable creation test between built-in Sonnet modules and TPUStrategy. (`619559ae`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add variable creation test between built-in Sonnet modules and TPUStrategy.
PiperOrigin-RevId: 252687703
Change-Id: I269b7dbeda7c7c9952eb09e23f4c4dca21d4123b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add variable creation test between built-in Sonnet modules and TPUStrategy.]


```

---

## 2019-06-11T12:12:17Z -- Raise an error when attempting to use optimizers with distribution strategies other than Sonnet Replicator. (`cfcada2d`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Raise an error when attempting to use optimizers with distribution strategies other than Sonnet Replicator.
PiperOrigin-RevId: 252597365
Change-Id: I638e8b33b530f61fb84c6d846c6fd51fc25859b1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Raise an error when attempting to use optimizers with distribution strategies other than Sonnet Replicator.]


```

---

## 2019-06-11T10:42:57Z -- Optimize name scoping for @snt.once methods. (`1e856639`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Optimize name scoping for @snt.once methods.
This shaves ~4.2?s of overhead (~1.7?s for name_scope.__init__ and the rest for
name_scope.__enter__). I am working in TF to (1) make name_scope re-entrant (so
we can do name_scope.__init__ once per module) and (2) to reduce the overhead in
__enter__.

Before this change, Sonnet was doing:

with self.name_scope:
if first_run:
f()

After this change we do:

if first_run:
with self.name_scope:
f()

PiperOrigin-RevId: 252587414
Change-Id: I75de8c3c074e5b6104b2dd53c540796c62fcf96e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Optimize name scoping for snt.once methods.]


```

---

## 2019-06-10T16:45:49Z -- CriticalSection around optimizer apply is not needed for ReplicaLocal variables. (`b4358e3c`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
CriticalSection around optimizer apply is not needed for ReplicaLocal variables.
PiperOrigin-RevId: 252422951
Change-Id: I764484026a491a561ada0d2e75ba4883d8f3fb3c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[CriticalSection around optimizer apply is not needed for ReplicaLocal variables.]


```

---

## 2019-06-10T15:52:37Z -- Add Python version check to colab notebook. (`8eba7d98`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Python version check to colab notebook.
PiperOrigin-RevId: 252413716
Change-Id: I171b44acf6532de3ce5e2ab4b166e439d4123491
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Python version check to colab notebook.]


```

---

## 2019-06-07T15:13:11Z -- Ensure that moving average variables in BatchNorm are correctly namescoped (`84d6b347`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure that moving average variables in BatchNorm are correctly namescoped
PiperOrigin-RevId: 252051372
Change-Id: Ic2e4a04c4ef67e7858e68a6a1cc95acf94a0013f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure that moving average variables in BatchNorm are correctly namescoped]


```

---

## 2019-06-07T13:42:49Z -- Add MLP on MNIST colab. (`c46edd16`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add MLP on MNIST colab.
PiperOrigin-RevId: 252040134
Change-Id: I2ac93bcb94174c55fbee58b5ac012c5fbafde76a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add MLP on MNIST colab.]


```

---

## 2019-06-07T12:22:40Z -- Print variable name when checkpoint tests fail (`ba0059d1`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Print variable name when checkpoint tests fail
PiperOrigin-RevId: 252031629
Change-Id: Ibfb316fdec8e003aa81edbd440a7b3c2b81dd890
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Print variable name when checkpoint tests fail]


```

---

## 2019-06-07T12:12:36Z -- Clarify that Sonnet supports NNs for many purposes. (`2f3f6823`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Clarify that Sonnet supports NNs for many purposes.
PiperOrigin-RevId: 252030793
Change-Id: Ifbf99bd6aa66f6a9a029448babd028b7c93a304a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Clarify that Sonnet supports NNs for many purposes.]


```

---

## 2019-06-06T13:42:09Z -- Improve Replicator tests for trainable variables. (`403f842f`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve Replicator tests for trainable variables.
PiperOrigin-RevId: 251841707
Change-Id: Ib59a4960599d9f6492d3c58c1c309ebe289ce9e4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve Replicator tests for trainable variables.]


```

---

## 2019-06-05T16:26:18Z -- Documented to Conv*DLSTM and Conv*DLSTM.__init__ (`7ebb9a30`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Documented to Conv*DLSTM and Conv*DLSTM.__init__
PiperOrigin-RevId: 251657105
Change-Id: I9c83073d475abfee056366d0a0a3c5d14d1d3b9b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Documented to ConvDLSTM and ConvDLSTM.__init__]


```

---

## 2019-06-05T12:34:15Z -- Added a role for linking to TensorFlow API docs (`9dcca34d`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added a role for linking to TensorFlow API docs
PiperOrigin-RevId: 251624539
Change-Id: Icfc2f4a885b9b932019660f5d504b3678916bbf7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added a role for linking to TensorFlow API docs]


```

---

## 2019-06-05T11:57:55Z -- Shuffled the order of the sections in the API docs (`d7d8fad8`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Shuffled the order of the sections in the API docs
PiperOrigin-RevId: 251620308
Change-Id: Ibed4ebf939feb71d802ea2853f081bf742934960
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Shuffled the order of the sections in the API docs]


```

---

## 2019-06-05T07:44:46Z -- Create trainable ReplicaLocalVariable by default even after upcoming TF bugfix. (`d78d5dd3`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Create trainable ReplicaLocalVariable by default even after upcoming TF bugfix.
PiperOrigin-RevId: 251592681
Change-Id: I7bdc0d458b11ac46be20fbf9ae6e9a76e3e8a634
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Create trainable ReplicaLocalVariable by default even after upcoming TF bugfix.]


```

---

## 2019-06-04T17:32:57Z -- Added a bit more structure to the API docs (`d9ce84be`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added a bit more structure to the API docs
PiperOrigin-RevId: 251466443
Change-Id: I3ade2c020e6e8feddd911fb9b8c17e3436f27a75
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added a bit more structure to the API docs]


```

---

## 2019-06-04T14:40:35Z -- Changes to Goldens tests to support modules which change state on forward call (`55a4541e`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Changes to Goldens tests to support modules which change state on forward call
PiperOrigin-RevId: 251436267
Change-Id: I8712c567d307bf45c7a1ab508b897469562a9645
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Changes to Goldens tests to support modules which change state on forward call]


```

---

## 2019-06-04T13:49:39Z -- Use snt.distribute.Replicator for the checkpoint tests instead of tf.distribute.MirroredStrategy (`515ae135`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use snt.distribute.Replicator for the checkpoint tests instead of tf.distribute.MirroredStrategy
Also requires adding a temporary fix for read_value on the variables

PiperOrigin-RevId: 251429682
Change-Id: If78191b838927ada7fb38291d7d93b461b88a004
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use snt.distribute.Replicator for the checkpoint tests instead of tf.distribute.MirroredStrategy]


```

---

## 2019-06-04T09:54:56Z -- Switched to KaTeX for math rendering (`f1c2a110`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Switched to KaTeX for math rendering
It's faster and more configurable than MathJax.

PiperOrigin-RevId: 251403540
Change-Id: I29d1af207502e5c14488130ca23217cdc54f8707
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Switched to KaTeX for math rendering]


```

---

## 2019-06-03T20:25:32Z -- Only initialize datasets once and cache the result. (`56f22a1e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Only initialize datasets once and cache the result.
PiperOrigin-RevId: 251296188
Change-Id: I4d6e5529f7fddd5517c77087c93e8641bdee25a0
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Only initialize datasets once and cache the result.]


```

---

## 2019-06-03T17:16:11Z -- Change moving averages to use int64 tensor for counter. (`bc966e7a`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change moving averages to use int64 tensor for counter.
This avoids saturation when the counter reaches the largest integer representable in floating-point.

PiperOrigin-RevId: 251256415
Change-Id: Id26f3439db55b60efb4ec26283e5d54d405b7c5a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change moving averages to use int64 tensor for counter.]


```

---

## 2019-06-03T17:06:25Z -- Autodoc snt.regularizers (`f9f9c860`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Autodoc snt.regularizers
Note that this change also adds some sane defaults for autodoc.

PiperOrigin-RevId: 251254611
Change-Id: I24b4e876b5b8a7908ca5749f138d4692b6bf5182
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Autodoc snt.regularizers]


```

---

## 2019-06-03T16:28:19Z -- Added Sphinx markup to recurrent (`e448c013`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added Sphinx markup to recurrent
PiperOrigin-RevId: 251247379
Change-Id: I2406f3491d80b48b908a16dabe56a7c98355e9be
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added Sphinx markup to recurrent]


```

---

## 2019-06-03T16:08:22Z -- Added readthedocs.yml (`2791eba2`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added readthedocs.yml
This makes (part of, see [*]) configuration versionable and also allows to
install dependencies from multiple requirements files.

[*]: https://docs.readthedocs.io/en/stable/config-file/v2.html#migrating-from-the-web-interface

PiperOrigin-RevId: 251243922
Change-Id: I6bc3fd89e9b72c4248f804f93a3b6f73d59f2fc8
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added readthedocs.yml]


```

---

## 2019-06-03T15:41:32Z -- Do not use imperative in recurrent docstrings (`2383586f`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Do not use imperative in recurrent docstrings
PiperOrigin-RevId: 251238895
Change-Id: I25aec8899b4b76f3da898abc6f9a2448d0d72409
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Do not use imperative in recurrent docstrings]


```

---

## 2019-06-03T15:40:45Z -- Unwrap decorated objects in linkcode_resolve (`926abfdf`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Unwrap decorated objects in linkcode_resolve
PiperOrigin-RevId: 251238765
Change-Id: Ic690a52331fbdbcbdcf9426cb560f829dbe5b8d4
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Unwrap decorated objects in linkcode_resolve]


```

---

## 2019-06-03T13:51:20Z -- Added L1, L2 and OffDiagonalOrthogonal regularizers (`6b53533c`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added L1, L2 and OffDiagonalOrthogonal regularizers
PiperOrigin-RevId: 251221873
Change-Id: If8cff87d144ce066f6fb1d969093d60c6e319eae
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added L1 L2 and OffDiagonalOrthogonal regularizers]


```

---

## 2019-06-03T10:24:29Z -- Add `snt.Embed(vocab_size=N)`. (`32e594a4`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add `snt.Embed(vocab_size=N)`.
PiperOrigin-RevId: 251198778
Change-Id: I961c430ceb7b0c7244d004a4e7746ce2ba1a228a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add snt.Embedvocab_sizeN.]


```

---

## 2019-06-03T10:05:29Z -- Loosen tollerances to make tests more stable. (`2a7c2ffe`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Loosen tollerances to make tests more stable.
PiperOrigin-RevId: 251196750
Change-Id: Id11abace612614f877f29141ea999e27cfc7ca25
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Loosen tollerances to make tests more stable.]


```

---

## 2019-06-03T09:43:18Z -- Remove extra quote. (`45a391b2`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove extra quote.
PiperOrigin-RevId: 251193548
Change-Id: Ia4eca4ce5f6a13212f23bf7b442723dedf6ef7f9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove extra quote.]


```

---

## 2019-05-31T11:43:14Z -- Store per-parameter optimizer variables in a list instead of a dict. (`e8b34891`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Store per-parameter optimizer variables in a list instead of a dict.
PiperOrigin-RevId: 250864940
Change-Id: Ie70fb72e69601635fc559c9c4a1df9f226cfb45d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Store per-parameter optimizer variables in a list instead of a dict.]


```

---

## 2019-05-31T11:08:27Z -- Add `variable_like` utility function. (`8b6687f6`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add `variable_like` utility function.
PiperOrigin-RevId: 250861900
Change-Id: I941faf5a2194e2f3adbea0e0a12cd90c3c206bba
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add variable_like utility function.]


```

---

## 2019-05-30T18:19:26Z -- Add Nesterov momentum to Momentum optimizer. (`78e96bd5`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Nesterov momentum to Momentum optimizer.
PiperOrigin-RevId: 250725134
Change-Id: Id8e2e0e79b13bb4fa20214bdfb85ae8edbb4f8fa
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Nesterov momentum to Momentum optimizer.]


```

---

## 2019-05-30T17:01:06Z -- Bumped tf-nightly version (`5f889e4d`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bumped tf-nightly version
The new version contains a fix needed for UnrolledLSTM.

PiperOrigin-RevId: 250707777
Change-Id: I9d7b17b708489b7e2639bb03652e91ddb240e90f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bumped tf-nightly version]


```

---

## 2019-05-30T16:48:44Z -- Rename `_create_parameters` to `_initialize`. (`2239a0b3`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Rename `_create_parameters` to `_initialize`.
We plan to use the same name consistently in Sonnet, but in some places (e.g.
optimizers) create_parameters would not make sense (here we are creating things
like moments associated with parameters).

PiperOrigin-RevId: 250705607
Change-Id: I797d08176f36fd2dfa89b4198fffd701642d00c5
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Rename _create_parameters to _initialize.]


```

---

## 2019-05-30T13:22:22Z -- Fix docstring typo. (`74db9bd4`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix docstring typo.
PiperOrigin-RevId: 250676701
Change-Id: Icc8c6c688c922b8327fd9f722bf519d319541bac
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix docstring typo.]


```

---

## 2019-05-30T10:06:15Z -- Add CI test for building docs. (`6ed1c685`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add CI test for building docs.
PiperOrigin-RevId: 250657535
Change-Id: I28bebd70367dddbb11a81cfd38053cc8b4939698
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add CI test for building docs.]


```

---

## 2019-05-30T09:57:20Z -- Update path before importing Sonnet. (`f9cc2b01`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update path before importing Sonnet.
PiperOrigin-RevId: 250656305
Change-Id: I5124fd91e1d3cc27b9931bf493cbab3dc34027c9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update path before importing Sonnet.]


```

---

## 2019-05-29T20:27:48Z -- Link to GitHub from Sphinx docs (`db67d7ec`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Link to GitHub from Sphinx docs
Note that this change also disables sphinx.ext.viewcode because
otherwise each symbol gets *two* "source" links.

PiperOrigin-RevId: 250554634
Change-Id: I82dd350cd7173d14c77106e1aa62d5e92c33727c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Link to GitHub from Sphinx docs]


```

---

## 2019-05-29T07:21:35Z -- Default visibility to private and expose just root build rule. (`d464bc38`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Default visibility to private and expose just root build rule.
PiperOrigin-RevId: 250436783
Change-Id: Ibdc671bde8e46b3d924933620b8721503ab10a4b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Default visibility to private and expose just root build rule.]


```

---

## 2019-05-29T07:18:05Z -- Add reference to tf.Module RFC. (`804f7d43`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add reference to tf.Module RFC.
h/t. tensorflow/community#56

PiperOrigin-RevId: 250436499
Change-Id: I9332d90f6331756f12907566af9a9e23f86aded8
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add reference to tf.Module RFC.]


```

---

## 2019-05-29T06:15:19Z -- Improved documentation for Replicator. (`200151a8`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improved documentation for Replicator.
Additionally to make the doctests pass I've added a temporary monkey-patch for
`SyncOnReadVariable.assign{,_add,_sub}` to make it work in replica context. We
intend to upstream this fix to Distribution Strategy soon.

PiperOrigin-RevId: 250430694
Change-Id: Iae79a3b163fade99813d8171e83785d39d8f3f99
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improved documentation for Replicator.]


```

---

## 2019-05-28T11:30:27Z -- Ensure name/device are restored in pickle tests. (`6e55d11c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure name/device are restored in pickle tests.
PiperOrigin-RevId: 250253860
Change-Id: I33a697f7b0e5225482f41931a5d5dbc1b13a6b44
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure name/device are restored in pickle tests.]


```

---

## 2019-05-28T11:30:21Z -- Move doctests into conformance. (`57d5177e`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move doctests into conformance.
PiperOrigin-RevId: 250253850
Change-Id: I754a310b6345e3fcf0cdf6f3e9ab5ef74b8edafe
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move doctests into conformance.]


```

---

## 2019-05-26T09:24:25Z -- Break goldens_test into conformance/${feature} tests. (`78f864fe`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Break goldens_test into conformance/${feature} tests.
A slightly annoying refactor for now, but worth it overall since the tests now
run faster and are easier to maintain.

PiperOrigin-RevId: 250030275
Change-Id: I43fea1735ca2d9f7e9af43820758b51b780a8c1c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Break goldens_test into conformance/feature tests.]


```

---

## 2019-05-24T13:01:58Z -- Add Replicator distribution strategy. (`75069254`)

**Author:** Peter Buchlovsky <peterbuchlovsky@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Replicator distribution strategy.
PiperOrigin-RevId: 249819860
Change-Id: Ic08bdf68ea63aa1b650a8068e94ef38edc0eab44
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Replicator distribution strategy.]


```

---

## 2019-05-24T11:50:16Z -- Remove a deprecation warning from using getargspec in Python 3. (`7648aee1`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove a deprecation warning from using getargspec in Python 3.
PiperOrigin-RevId: 249813135
Change-Id: I26f0f53079c6fe537cf7676e66007d8a4aba4f10
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove a deprecation warning from using getargspec in Python 3.]


```

---

## 2019-05-24T09:40:35Z -- Copy over docs from ConvND{,Transpose} to fixed spatial dims versions. (`81427016`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Copy over docs from ConvND{,Transpose} to fixed spatial dims versions.
PiperOrigin-RevId: 249800837
Change-Id: Ia42641a9a6f4abb063f52212c1849e9001cdc241
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Copy over docs from ConvNDTranspose to fixed spatial dims versions.]


```

---

## 2019-05-23T16:22:28Z -- Delete src so people can't use `import sonnet.v2 as snt; snt.src.*`. (`e684b765`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Delete src so people can't use `import sonnet.v2 as snt; snt.src.*`.
PiperOrigin-RevId: 249657388
Change-Id: I379016fdfe8990cd0ad5b789e1c02d9d21a28e88
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Delete src so people cant use import sonnet.v2 as snt snt.src..]


```

---

## 2019-05-23T10:07:27Z -- Fix to ensure that the correct axis is being used in Batch Norm (`25b98bcc`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix to ensure that the correct axis is being used in Batch Norm
When using channels last batch norm for which the channel_index is -1 the axis being used was incorrect

PiperOrigin-RevId: 249611659
Change-Id: Ib57f04bf68723b5e6549de96a25fbbe5063f2279
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix to ensure that the correct axis is being used in Batch Norm]


```

---

## 2019-05-23T08:47:07Z -- Move MANIFEST.in to the root directory. (`cedd915c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move MANIFEST.in to the root directory.
PiperOrigin-RevId: 249602363
Change-Id: I85b348156173b80ef23294ae5f0aceb3368c3945
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move MANIFEST.in to the root directory.]


```

---

## 2019-05-22T17:09:01Z -- Initial Sphinx documentation. (`c749f63c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Initial Sphinx documentation.
Currently this only contains the auto-generated API docs, but over time we can
extend and improve this to include additional documentation and tutorials that
are not in docstrings.

To build and view the docs run the following and navigate to
http://localhost:8888:

$ cd docs/
$ pip install -r requirements.txt
$ cd _build/html
$ python -m http.server 8888

We will automate the process of generating and pushing the docs to an
appropriate destination (e.g. gh-pages and/or readthedocs) when commits are
merged or releases are tagged.

PiperOrigin-RevId: 249470120
Change-Id: I5d0de111a19320e9c52e5d08aa6e395aeff68022
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Initial Sphinx documentation.]


```

---

## 2019-05-21T15:00:59Z -- Internal change. (`136fedaf`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 249250118
Change-Id: I568b5827ac7a2cda2b0bdef69b0442df91699b54
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2019-05-21T14:13:48Z -- Tidy up example. (`5bd8a1da`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Tidy up example.
1) Use `as_supervised` to get pairs instead of dict.
2) Move `tf.function` around epoch (dataset iteration now supported).
3) Use Sonnet optimizer.

[Epoch 0] train loss: 0.02844, test acc: 97.84% (216 wrong)
[Epoch 1] train loss: 0.01115, test acc: 98.19% (181 wrong)
[Epoch 2] train loss: 0.05174, test acc: 98.25% (175 wrong)
[Epoch 3] train loss: 0.01407, test acc: 98.49% (151 wrong)
[Epoch 4] train loss: 0.04556, test acc: 98.48% (152 wrong)

PiperOrigin-RevId: 249243773
Change-Id: I14615b4c8688c0775dda7fc939aa5cc647bf2ee0
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Tidy up example.]


```

---

## 2019-05-21T10:24:55Z -- Merge pull request #130 from deepmind:tomhennigan-patch-1 (`5f7bca5c`)

**Author:** Copybara-Service <copybaraservice@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #130 from deepmind:tomhennigan-patch-1
PiperOrigin-RevId: 249219816
Change-Id: Ia338021c8a67b4cc748ac01a87d262d01220cdfd
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 130 from deepmind:tomhennigan-patch-1]


```

---

## 2019-05-21T09:46:40Z -- Add logo to read me. (`ed7f318a`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add logo to read me.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add logo to read me.]


```

---

## 2019-05-20T18:03:55Z -- Add correct import statement to README.md (`e73f9375`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add correct import statement to README.md
PiperOrigin-RevId: 249084718
Change-Id: Iea8eddba539fc93bb8518fb552c546cbc7107afb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add correct import statement to README.md]


```

---

## 2019-05-20T17:55:24Z -- Internal changes (`d275209b`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal changes
PiperOrigin-RevId: 249082281
Change-Id: I5a3b31bf66107dd21cf2a1c5d01e48fed6594185
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal changes]


```

---

## 2019-05-20T15:34:50Z -- Bump TensorFlow nightly version. (`9fd38b0c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump TensorFlow nightly version.
PiperOrigin-RevId: 249054660
Change-Id: Ibc12ed295a6475ebdc967a4b2156ee07a600b5c2
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump TensorFlow nightly version.]


```

---

## 2019-05-20T15:16:16Z -- Merge pull request #129 from superbobry:patch-1 (`a29e5801`)

**Author:** Copybara-Service <copybaraservice@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #129 from superbobry:patch-1
PiperOrigin-RevId: 249051519
Change-Id: Ie0ce0863c78685390debea37e4a5cecca64bd2fb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 129 from superbobry:patch-1]


```

---

## 2019-05-20T08:42:53Z -- Add some type annotations to snt.Module and friends. (`f1188ef3`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add some type annotations to snt.Module and friends.
Internally we rely on a patched version of Python 2.7 [0] to backport type
annotations. Outside Alphabet we only support Python >=3.6.

[0] https://github.com/google/pytype/blob/master/2.7_patches/

PiperOrigin-RevId: 249005879
Change-Id: I408e25134fb6921a119270df0602d39749e3f41f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add some type annotations to snt.Module and friends.]


```

---

## 2019-05-20T07:26:52Z -- Allow constructor to be annotated with `@snt.no_name_scope`. (`7a26a5e4`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow constructor to be annotated with `@snt.no_name_scope`.
PiperOrigin-RevId: 248996583
Change-Id: Ic6fabae51f6f87987f1f134d0e609b90b36cafad
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow constructor to be annotated with snt.no_name_scope.]


```

---

## 2019-05-17T16:24:06Z -- Add documentation from docstrings to docs folder (`a3e84e7d`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add documentation from docstrings to docs folder
PiperOrigin-RevId: 248733148
Change-Id: Ibd6c92cdbbbe2814e1668f49d94a368a948e2f3d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add documentation from docstrings to docs folder]


```

---

## 2019-05-17T16:11:14Z -- Indent documentation to fix formatting in rendered output. (`c220de5c`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Indent documentation to fix formatting in rendered output.
PiperOrigin-RevId: 248730956
Change-Id: I868cfd4acc1404877b4fa04f7bc3cd23ba88fcb1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Indent documentation to fix formatting in rendered output.]


```

---

## 2019-05-16T16:19:38Z -- Internal change. (`c7299210`)

**Author:** Chris Jones <chrisjones@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 248540274
Change-Id: I93e8e48522a8343daeda132c8f6287807c8a0e26
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2019-05-16T14:29:58Z -- Added a variable to the Counter core (`e1860e56`)

**Author:** Sergei Lebedev <sergeilebedev@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added a variable to the Counter core
This allows to catch variable-related issues in unroll functions.

PiperOrigin-RevId: 248523015
Change-Id: Iddc3b094ad0d2ed19bd2bee2f067c4133fc02b6e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added a variable to the Counter core]


```

---

## 2019-05-16T06:28:55Z -- Allow numeric difference in test. (`5408d9c2`)

**Author:** Sonnet Contributor <sonnetcontributor@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow numeric difference in test.
PiperOrigin-RevId: 248472685
Change-Id: I13a2eac72e451a8d7268979a9847f9688c012dcb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow numeric difference in test.]


```

---

## 2019-05-15T14:48:47Z -- Add `snt.Bias`. (`8fc134dc`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add `snt.Bias`.
Mirroring Sonnet 1, this class infers the bias shape from its input or supports
an explicit bias shape (which must be broadcastable with the input).

PiperOrigin-RevId: 248331959
Change-Id: Ie3105d8083156e308053f3fe7a695bc4a22f3007
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add snt.Bias.]


```

---

## 2019-05-14T16:19:37Z -- Remove force to python 3.6 as old way of doing, instead must have it as default python 3 on system (`a81b12b1`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove force to python 3.6 as old way of doing, instead must have it as default python 3 on system
Also run setup.py as part of the test.sh

PiperOrigin-RevId: 248149653
Change-Id: Iaee55c25241a01d1cea2242a9173aa4031ac77d0
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove force to python 3.6 as old way of doing instead must have it as default python 3 on system]


```

---

## 2019-05-14T15:10:52Z -- Use pyenv to bring in python 3.6.1 (`aaa05de6`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use pyenv to bring in python 3.6.1
PiperOrigin-RevId: 248138506
Change-Id: Idd2772334dc5cc3e2dfc2480a1ba6ce15166534a
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use pyenv to bring in python 3.6.1]


```

---

## 2019-05-14T14:53:58Z -- Force Python 3.6 (`b7e3c0e1`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Force Python 3.6
PiperOrigin-RevId: 248135750
Change-Id: Iac28cce18e503c528177d6f6651e44aa1df7151d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Force Python 3.6]


```

---

## 2019-05-14T14:09:18Z -- Add changes to test.sh to give more information for debugging (`9dd27e2e`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add changes to test.sh to give more information for debugging
PiperOrigin-RevId: 248130115
Change-Id: I10b7261ad5d895bcc2128eabef9591a3b1964253
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add changes to test.sh to give more information for debugging]


```

---

## 2019-05-14T10:46:03Z -- Internal changes (`df8ce6ef`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal changes
PiperOrigin-RevId: 248108073
Change-Id: Ib64a7d364d025ec258b664512666b91748973c24
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal changes]


```

---

## 2019-05-13T19:24:23Z -- Additional checkpoint tests with distribution strategy. (`1a4e14ee`)

**Author:** Tom Hennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Additional checkpoint tests with distribution strategy.
Summary:

- Use virtual devices API to do multi-GPU testing even with a single GPU.
- Make assert_consumed explicit in TestCheckpoint#restore_latest.
- Test restore from golden.
- Test restore from non-distributed model.
- Test save/restore cycle.
- Test restore on create.
- Test restore on create in replica context.

PiperOrigin-RevId: 247986699
Change-Id: I46f3da2ae1201eddd779a5634ef147be32e38f92
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Additional checkpoint tests with distribution strategy.]


```

---

## 2019-05-13T13:12:01Z -- Remove obsolete required packages now we use requirements.txt (`9956bbf3`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove obsolete required packages now we use requirements.txt
PiperOrigin-RevId: 247921437
Change-Id: I3cea10b9864f54f394582742402d50c7d8d47b42
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove obsolete required packages now we use requirements.txt]


```

---

## 2019-05-13T12:20:56Z -- Fix build rules (`ac19d93b`)

**Author:** Tamara Norman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix build rules
PiperOrigin-RevId: 247916555
Change-Id: Ibcf44cb5f2e6d4a6c1c671230afc3b9e99cd447f
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix build rules]


```

---

## 2019-05-13T10:24:02Z -- Add copyright header to test script. (`9d402730`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add copyright header to test script.
PiperOrigin-RevId: 247905899
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add copyright header to test script.]


```

---

## 2019-05-10T11:30:16Z -- Move common optimizer utility functions into optimizer_utils.py. (`ec547f59`)

**Author:** petebu <petebu@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move common optimizer utility functions into optimizer_utils.py.
PiperOrigin-RevId: 247589803
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move common optimizer utility functions into optimizer_utils.py.]


```

---

## 2019-05-10T11:18:09Z -- Avoid duplicating requirements in setup.py and test.sh. (`3cb8b12e`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Avoid duplicating requirements in setup.py and test.sh.
PiperOrigin-RevId: 247588923
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Avoid duplicating requirements in setup.py and test.sh.]


```

---

## 2019-05-10T10:19:07Z -- Correctly parse Sonnet __version__ by execing it. (`a0cc940a`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Correctly parse Sonnet __version__ by execing it.
PiperOrigin-RevId: 247584044
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Correctly parse Sonnet __version__ by execing it.]


```

---

## 2019-05-10T10:01:32Z -- Allow setup.py to run without pre-installing Sonnet deps. (`2504a847`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow setup.py to run without pre-installing Sonnet deps.
PiperOrigin-RevId: 247581728
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow setup.py to run without pre-installing Sonnet deps.]


```

---

## 2019-05-10T08:34:13Z -- Add missing line continuation. (`68d76893`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add missing line continuation.
PiperOrigin-RevId: 247572414
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add missing line continuation.]


```

---

## 2019-05-09T14:52:44Z -- Add initial documentation for Sonnet 2. (`322cd5a6`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add initial documentation for Sonnet 2.
PiperOrigin-RevId: 247424280
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add initial documentation for Sonnet 2.]


```

---

## 2019-05-09T14:27:22Z -- Add test.sh to Sonnet (`5c7873f3`)

**Author:** tamaranorman <tamaranorman@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add test.sh to Sonnet
PiperOrigin-RevId: 247420802
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add test.sh to Sonnet]


```

---

## 2019-05-09T13:35:57Z -- Run checkpoint tests with TPUStrategy. (`a0464dc4`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run checkpoint tests with TPUStrategy.
PiperOrigin-RevId: 247413794
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run checkpoint tests with TPUStrategy.]


```

---

## 2019-05-09T10:13:39Z -- Initial export of Sonnet 2. (`48720f12`)

**Author:** DeepMind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Initial export of Sonnet 2.
Sonnet 2 is a re-write of Sonnet for TensorFlow 2.0, it is built on top of
`tf.Module` enabling a simple and researcher friendly interface to TensorFlow.

Please note that Sonnet 2 is currently alpha. We would love to have you use it
as an early adopter, but please make sure to test that the built in modules
behave as you expect. We would love your feedback and read all the issues posted
on GitHub.

```

2S2
22SSSS22
22S2SSSSSSSS22
22S2SSS2    SSSSSS22
SS2SSSSS        2SSSSSS2
2SSSSSS22        2SSSSSS2
2S22        SSS2SSS2S        2SS2SSS
2SSSSSS2        SSSS2SS22        SSSSS2
SSSS2SSS        SSSSSSSSS            S2
2222   SSSSSSSS        SSS2SSSS2      2S2S2
222222S   SSSSSSS2      2SSS2SS2   S2S2S22S
2222222222   SSSSSS22S2S2S2S2   S22S22S2
222S S22222S2   SS2SSS2SS2   S22S2S2
2222    2222222S   SSS2   S2S2S2S        S2
2222       S222222     SS2S2222        S2S2
22222         222222  2S2222        S2S2S22
SS222222         222  2S22       S222S2S222
2222222S           2S22     22S2S2222S22
S2222222        2S22  2S2S2222  2S2S2
S        S222222S     2S2S2S22S22     2S222
22S         S222222S  2S22S2S2        22S22
222222         S2222  2S2S2         22S2S22
22222222S       S222  22S        22S2S2S
2S222222S    S222          22S2S2SS
222222222 S222       22S2S2SS
2S222222222    222S22SS2
2S222222  2S2S22SS
2S222  22SS2
2S  22
```

c.f.: deepmind/sonnet#117

PiperOrigin-RevId: 247390692
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Initial export of Sonnet 2.]


```

---

## 2019-04-12T14:07:05Z -- Bump version to 1.32 and add final changes to changelog. (`00612ca3`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump version to 1.32 and add final changes to changelog.
This also adds an __init__ to the protos directory to make bazel pick it up.

PiperOrigin-RevId: 243257844
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump version to 1.32 and add final changes to changelog.]


```

---

## 2019-04-11T17:58:40Z -- Expose Sonnet 2 via `import sonnet.v2 as snt`. (`1ff44fd4`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose Sonnet 2 via `import sonnet.v2 as snt`.
PiperOrigin-RevId: 243101231
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose Sonnet 2 via import sonnet.v2 as snt.]


```

---

## 2019-04-11T14:08:51Z -- Allow named argument 'outputs' in calls to Sonnet modules. (`f8552f4b`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow named argument 'outputs' in calls to Sonnet modules.
PiperOrigin-RevId: 243062408
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow named argument outputs in calls to Sonnet modules.]


```

---

## 2019-04-11T08:03:29Z -- Python 3 all the things. (`bd56b26b`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Python 3 all the things.
PiperOrigin-RevId: 243021127
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Python 3 all the things.]


```

---

## 2019-04-10T16:52:50Z -- Remove experimental references from documentation. (`54a9a10f`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove experimental references from documentation.
PiperOrigin-RevId: 242887298
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove experimental references from documentation.]


```

---

## 2019-04-09T14:59:06Z -- Updated changelog for revised 1.32 release. (`63f36803`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Updated changelog for revised 1.32 release.
PiperOrigin-RevId: 242666638
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Updated changelog for revised 1.32 release.]


```

---

## 2019-04-09T09:32:45Z -- internal change (`74e96ec4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 242629869
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-04-05T09:24:48Z -- internal change (`ed3f4f61`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 242093580
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-04-02T14:26:39Z -- internal change (`961d7a78`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 241523718
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-03-29T10:18:51Z -- internal change (`12f10ed2`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 240950108
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-03-28T12:12:51Z -- Sonnet version update produced on Wednesday, 27. March 2019 (`90dcf838`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Wednesday, 27. March 2019
PiperOrigin-RevId: 240752885
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Wednesday 27. March 2019]


```

---

## 2019-03-26T10:25:34Z -- Internal refactor (`843ed0a4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal refactor
PiperOrigin-RevId: 240314998
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal refactor]


```

---

## 2019-02-12T17:11:30Z -- Sonnet version update produced on Tuesday, 12. February 2019 (`c2b3edac`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 12. February 2019
PiperOrigin-RevId: 233617272
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 12. February 2019]


```

---

## 2019-02-12T17:08:14Z -- Add mkdocs yaml file. (`9c681200`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add mkdocs yaml file.
PiperOrigin-RevId: 233616884
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add mkdocs yaml file.]


```

---

## 2019-02-12T09:51:42Z -- internal change (`bcec0d41`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 233565498
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-02-08T18:17:33Z -- Support lower precision inputs. (`b12c680c`)

**Author:** jwrae <jwrae@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support lower precision inputs.
PiperOrigin-RevId: 233081564
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support lower precision inputs.]


```

---

## 2019-02-08T10:59:16Z -- internal change (`84e800fd`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 233030785
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-02-05T18:15:48Z -- A class to linear transform the concatenation of a list of Tensors. It ensures the relative importance of all inputs are similar even if they have very different sizes. (`d11f9c4f`)

**Author:** sracaniere <sracaniere@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
A class to linear transform the concatenation of a list of Tensors. It ensures the relative importance of all inputs are similar even if they have very different sizes.
PiperOrigin-RevId: 232509624
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[A class to linear transform the concatenation of a list of Tensors. It ensures the relative importance of all inputs are similar even if they have very different sizes.]


```

---

## 2019-02-05T09:04:59Z -- internal change (`62b0f6e2`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 232439572
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-02-04T18:06:54Z -- Update protobuf dependency. (`03d9aef0`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update protobuf dependency.
PiperOrigin-RevId: 232317599
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update protobuf dependency.]


```

---

## 2019-01-29T19:40:35Z -- Sonnet version update produced on Tuesday, 29. January 2019 (`381b630e`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 29. January 2019
PiperOrigin-RevId: 231442829
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 29. January 2019]


```

---

## 2019-01-29T09:21:56Z -- internal change (`e1f82837`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 231365216
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-01-28T11:00:01Z -- Internal change (`8b4942f1`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 231186348
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2019-01-26T22:46:09Z -- Internal change. (`ae7e0010`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 231069857
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2019-01-26T21:01:19Z -- Make module/connection stacks thread local. (`674007ec`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make module/connection stacks thread local.
PiperOrigin-RevId: 231065224
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make module/connection stacks thread local.]


```

---

## 2019-01-25T21:36:51Z -- Internal Change. (`f1384921`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal Change.
PiperOrigin-RevId: 230959989
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal Change.]


```

---

## 2019-01-25T12:22:35Z -- internal change (`3a9e9046`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 230881819
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-01-24T13:19:44Z -- Allow control over max pondering steps. (`eb460863`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow control over max pondering steps.
PiperOrigin-RevId: 230704013
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow control over max pondering steps.]


```

---

## 2019-01-24T11:23:21Z -- Add hyperlinks to documentation. (`f451e9a8`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add hyperlinks to documentation.
PiperOrigin-RevId: 230691280
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add hyperlinks to documentation.]


```

---

## 2019-01-24T11:23:06Z -- Fix headers in installation instructions. (`e478c60a`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix headers in installation instructions.
PiperOrigin-RevId: 230691260
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix headers in installation instructions.]


```

---

## 2019-01-22T09:44:49Z -- internal change (`6c53258a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 230302480
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-01-08T12:08:30Z -- internal change (`249c817b`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 228307386
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2019-01-07T12:19:34Z -- Increase test size to accommodate sanitizers (`675e849d`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Increase test size to accommodate sanitizers
PiperOrigin-RevId: 228141037
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Increase test size to accommodate sanitizers]


```

---

## 2019-01-06T17:14:18Z -- Remove `@snt.experimental.reuse_vars`. (`2f7a52fb`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove `@snt.experimental.reuse_vars`.
Please use `@snt.reuse_variables` instead!

PiperOrigin-RevId: 228063197
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove snt.experimental.reuse_vars.]


```

---

## 2019-01-03T22:16:42Z -- Allowing Conv2D to accept unicode, in addition to str. (`a9c6a8d9`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allowing Conv2D to accept unicode, in addition to str.
PiperOrigin-RevId: 227748726
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allowing Conv2D to accept unicode in addition to str.]


```

---

## 2019-01-03T17:58:37Z -- Add MLP MNIST example. (`0bfb4041`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add MLP MNIST example.
PiperOrigin-RevId: 227704815
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add MLP MNIST example.]


```

---

## 2019-01-03T17:52:59Z -- Add wrapt to required packages. (`e95ba5c7`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add wrapt to required packages.
Fixes https://github.com/deepmind/sonnet/issues/115

PiperOrigin-RevId: 227703904
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add wrapt to required packages.]


```

---

## 2019-01-02T08:34:44Z -- internal change (`a12ce4d8`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 227485581
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2018-12-29T00:05:30Z -- Put reused variables in _all_variables when using nested modules. (`bbdf2ae8`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Put reused variables in _all_variables when using nested modules.
It was possible before to end up in an inconsistent state if inside `_build`
`ParentModule` assigned `self.child = Child()` and in your training loop you
used `mod.child.variables` (or `get_all_variables()`):

```
mod = ParentModule()
for record in inputs:
with tf.GradientTape() as tape:
outs = mod(record)
vars = mod.child.variables  # After first iteration this would be empty.
grads = tape.gradient(outs, vars)
```

While I suspect this sort of thing would be more common in eager mode, the bug
still exists in graph mode.

PiperOrigin-RevId: 227178237
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Put reused variables in _all_variables when using nested modules.]


```

---

## 2018-12-28T09:12:07Z -- internal change (`91bab7be`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 227108833
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2018-12-27T11:56:34Z -- Enable Python 3 in Sonnet's py_binary rules. (`fcf20ed1`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Enable Python 3 in Sonnet's py_binary rules.
PiperOrigin-RevId: 227011720
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Enable Python 3 in Sonnets py_binary rules.]


```

---

## 2018-12-26T23:30:02Z -- Remove some dependencies on internal TensorFlow symbols. (`e0988b1a`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove some dependencies on internal TensorFlow symbols.
PiperOrigin-RevId: 226960072
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove some dependencies on internal TensorFlow symbols.]


```

---

## 2018-12-21T13:30:08Z -- internal change (`0400b3dc`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 226476878
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2018-12-19T16:48:03Z -- internal change (`fe13d132`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 226179552
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2018-12-19T11:30:40Z -- Sonnet version update produced on Wednesday, 19. December 2018 (`79d920c7`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Wednesday, 19. December 2018
PiperOrigin-RevId: 226148846
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Wednesday 19. December 2018]


```

---

## 2018-12-18T17:26:09Z -- Update changelog for version 1.28 (`bb2e3827`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog for version 1.28
PiperOrigin-RevId: 226008459
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog for version 1.28]


```

---

## 2018-12-18T16:56:56Z -- FIX: setup.py.tmpl referenced wrong package name (`39e817be`)

**Author:** TR <tr@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
FIX: setup.py.tmpl referenced wrong package name
setup.py.tmpl referenced "tensor-probability-gpu" instead of "tensorflow-probability-gpu".
this caused installation via pip to fail since dm-sonnet-gpu==1.25

GIT_ORIGIN_REV_ID=b18ca77dd73795d5c18dbf2a1e895471493c15f2
PiperOrigin-RevId: 226004212
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[FIX: setup.py.tmpl referenced wrong package name]


```

---

## 2018-12-12T15:27:14Z -- internal change (`8b557d84`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 225181012
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2018-12-11T14:50:30Z -- ConvNet2D{Transpose}: Filter the kwargs passed to the normalizer. (`a35746dd`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
ConvNet2D{Transpose}: Filter the kwargs passed to the normalizer.
This makes it possible to connect the module with is_training=True, even if the
normalization module selected is something which does not support that (eg
LayerNorm). Normal practice would be to call the module with is_training inside
a try block, catch any errors and then reconnect without is_training. However,
that will generally have created some variables internally, so global
(tf.Graph-level) state has been changed, producing errors.

This solution works for any normalization module which has a signature that
does not contain **kwargs, because that makes it impossible to introspect over
whether the flag is supported or not.

PiperOrigin-RevId: 224994092
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[ConvNet2DTranspose: Filter the kwargs passed to the normalizer.]


```

---

## 2018-12-05T18:12:02Z -- 1. Fix the sequence reading order for backward unroll. The order should be (`0c8a946c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
1. Fix the sequence reading order for backward unroll. The order should be
decreasing but was increasing.

2. Fix RNN unroll by feeding correct state (the previous state) to each unroll
step. The state was always the initial state.

PiperOrigin-RevId: 224174351
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[1. Fix the sequence reading order for backward unroll. The order should be]


```

---

## 2018-11-30T16:06:24Z -- Logging utility function, to be used from inside a module. (`8ba30c4a`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Logging utility function, to be used from inside a module.
A kwarg `verbose=True` must be provided to the function for it to actually print, meaning users don't need to fill their module code with if statements.

Currently just logs the provided information, preceded by full module path.

PiperOrigin-RevId: 223516389
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Logging utility function to be used from inside a module.]


```

---

## 2018-11-30T13:24:30Z -- Add `remove_unsupported_kwargs()` which can filter a set of potential kwargs by whether they are supported by a module. (`9b1503a7`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add `remove_unsupported_kwargs()` which can filter a set of potential kwargs by whether they are supported by a module.
If the function has **kwargs in the signature, we assume all kwargs are valid.

PiperOrigin-RevId: 223500517
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add remove_unsupported_kwargs which can filter a set of potential kwargs by whether they are supported by a module.]


```

---

## 2018-11-28T16:59:38Z -- Remove references to `tf.contrib.rnn.RNNCell` in the RNNCore docstring. (`f20e0a6a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove references to `tf.contrib.rnn.RNNCell` in the RNNCore docstring.
While RNNCore used to inherit from RNNCell, it doesn't any longer and the docstring needed updating.

PiperOrigin-RevId: 223176347
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove references to tf.contrib.rnn.RNNCell in the RNNCore docstring.]


```

---

## 2018-11-28T16:50:47Z -- Re-enable bfloat16 test (`5d2e01c1`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Re-enable bfloat16 test
PiperOrigin-RevId: 223175090
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Re-enable bfloat16 test]


```

---

## 2018-11-28T14:51:17Z -- Add supports_kwargs() function. (`0b660f95`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add supports_kwargs() function.
This allows testing whether some callable, either a function / method / object, supports a list of keyword args. If an object is provided, object.__call__ is checked which maps to _build for Sonnet modules.

This can be used for checking whether kwargs like `is_training` are supported by a given module.

PiperOrigin-RevId: 223158721
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add supports_kwargs function.]


```

---

## 2018-11-28T13:40:50Z -- gated_rnn_test: Make GRUTest also inherit from parameterized for consistency. (`3216e4b5`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
gated_rnn_test: Make GRUTest also inherit from parameterized for consistency.
PiperOrigin-RevId: 223151892
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[gated_rnn_test: Make GRUTest also inherit from parameterized for consistency.]


```

---

## 2018-11-27T19:34:14Z -- Make recurrent dropout / zoneout tests less extreme. (`78d9437f`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make recurrent dropout / zoneout tests less extreme.
PiperOrigin-RevId: 223027131
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make recurrent dropout / zoneout tests less extreme.]


```

---

## 2018-11-27T16:53:09Z -- Use more specific assertions in util_test. (`7552f411`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use more specific assertions in util_test.
PiperOrigin-RevId: 222998330
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use more specific assertions in util_test.]


```

---

## 2018-11-23T10:13:22Z -- Disable mod.get_variables() in eager mode when using defun. (`6d464147`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Disable mod.get_variables() in eager mode when using defun.
PiperOrigin-RevId: 222602705
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Disable mod.get_variables in eager mode when using defun.]


```

---

## 2018-11-22T23:31:22Z -- Replaced deprecated tf.create_partitioned_variables with tf.get_variable (`7217be12`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Replaced deprecated tf.create_partitioned_variables with tf.get_variable
PiperOrigin-RevId: 222569043
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Replaced deprecated tf.create_partitioned_variables with tf.get_variable]


```

---

## 2018-11-20T17:45:06Z -- Sonnet version update produced on Tuesday, 20. November 2018 (`436f7949`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 20. November 2018
PiperOrigin-RevId: 222260709
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 20. November 2018]


```

---

## 2018-11-20T16:50:53Z -- Update Changelog for version 1.27 (`289e2272`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update Changelog for version 1.27
PiperOrigin-RevId: 222252851
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update Changelog for version 1.27]


```

---

## 2018-11-20T15:32:52Z -- scale by key_size in relational memory _multihead_attention (`989e3978`)

**Author:** Dr. Kashif Rasul <drkashifrasul@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
scale by key_size in relational memory _multihead_attention
GIT_ORIGIN_REV_ID=b92d54997e7df890458b672540c4f9832da9e8aa
PiperOrigin-RevId: 222243055
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[scale by key_size in relational memory _multihead_attention]


```

---

## 2018-11-19T17:06:49Z -- Make SkipConnectionCore and ResidualCore call the initial_state/zero_state methods of the base core. (`9efc4321`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make SkipConnectionCore and ResidualCore call the initial_state/zero_state methods of the base core.
PiperOrigin-RevId: 222085553
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make SkipConnectionCore and ResidualCore call the initial_state/zero_state methods of the base core.]


```

---

## 2018-11-19T14:13:35Z -- Rename the 'axes' argument in the snt.LayerNorm constructor to 'axis' to be consistent with sonnet's BatchNorm constructor. (`8a6d51c7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Rename the 'axes' argument in the snt.LayerNorm constructor to 'axis' to be consistent with sonnet's BatchNorm constructor.
Also modified the checks (and docs) such that axis can alternatively be a scalar int instead of requiring a list for convenience.

PiperOrigin-RevId: 222064990
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Rename the axes argument in the snt.LayerNorm constructor to axis to be consistent with sonnets BatchNorm constructor.]


```

---

## 2018-11-12T17:59:34Z -- Copy signature of _build to __call__. (`eace9e7d`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Copy signature of _build to __call__.
PiperOrigin-RevId: 221109661
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Copy signature of _build to __call__.]


```

---

## 2018-11-12T15:15:48Z -- Backwards-compatibility fix for _ConvND.padding, following cl/221079656 which introduced different padding types per dimension. (`f0ae8ac5`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Backwards-compatibility fix for _ConvND.padding, following cl/221079656 which introduced different padding types per dimension.
layer.padding will now return a single padding type where it's the same across all dimensions; .paddings can be used to get the padding for each dimension separately.

PiperOrigin-RevId: 221087200
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Backwards-compatibility fix for _ConvND.padding following cl/221079656 which introduced different padding types per dimension.]


```

---

## 2018-11-07T13:18:48Z -- Print the type (legacy or resource) of Tensorflow variables in snt.log_variables(). (`ee326a92`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Print the type (legacy or resource) of Tensorflow variables in snt.log_variables().
PiperOrigin-RevId: 220446611
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Print the type legacy or resource of Tensorflow variables in snt.log_variables.]


```

---

## 2018-11-05T17:27:20Z -- Change some formatting, based on the automatic formatter. (`5f28cec9`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change some formatting, based on the automatic formatter.
PiperOrigin-RevId: 220116426
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change some formatting based on the automatic formatter.]


```

---

## 2018-11-05T15:58:08Z -- Make ConvNet2D more flexible in what normalization scheme is used. (`667f06e0`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make ConvNet2D more flexible in what normalization scheme is used.
The use_batch_norm and batch_norm_config flags are deprecated and will be removed in the future.

Note that for backwards compatibility, the normalization modules will still be built with the name 'batch_norm_{layer_index}'. Old checkpoints will still load.

PiperOrigin-RevId: 220102133
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make ConvNet2D more flexible in what normalization scheme is used.]


```

---

## 2018-11-02T10:27:44Z -- Use initializer with stddev=1 for sonnet.Embed. (`c646148a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use initializer with stddev=1 for sonnet.Embed.
PiperOrigin-RevId: 219775902
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use initializer with stddev1 for sonnet.Embed.]


```

---

## 2018-10-31T13:14:34Z -- Allow `LayerNorm` to accept >2D input (`3c85a7c7`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow `LayerNorm` to accept >2D input
Previously >2D input would be an error, whereas now it will normalize over all non-batch dimensions. No code which previously didn't throw an error should have changed behaviour.

PiperOrigin-RevId: 219461741
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow LayerNorm to accept 2D input]


```

---

## 2018-10-30T11:39:52Z -- A test erroneously suggested use_batch_norm accepts iterables of booleans. Fix (`729aedf0`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
A test erroneously suggested use_batch_norm accepts iterables of booleans. Fix
the test and notify users who were using snt.ConvNet2D unknowingly incorrectly.

PiperOrigin-RevId: 219278708
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[A test erroneously suggested use_batch_norm accepts iterables of booleans. Fix]


```

---

## 2018-10-29T13:04:00Z -- Internal change (`b4a6f8ab`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 219116281
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2018-10-22T11:10:41Z -- Sonnet version update produced on Monday, 22. October 2018 (`d70b5311`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 22. October 2018
PiperOrigin-RevId: 218144877
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 22. October 2018]


```

---

## 2018-10-19T16:39:11Z -- Check dependencies before importing rest of library (`53aac92b`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Check dependencies before importing rest of library
PiperOrigin-RevId: 217882721
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Check dependencies before importing rest of library]


```

---

## 2018-10-16T13:42:58Z -- Sonnet version update produced on Tuesday, 16. October 2018 (`087cb9d9`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 16. October 2018
PiperOrigin-RevId: 217309141
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 16. October 2018]


```

---

## 2018-10-16T12:57:05Z -- Update changelog for 1.25 (`4fe21ce0`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog for 1.25
PiperOrigin-RevId: 217304436
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog for 1.25]


```

---

## 2018-10-12T16:34:50Z -- Change Sonnet to depend on tensorflow_probability (`3f217513`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change Sonnet to depend on tensorflow_probability
PiperOrigin-RevId: 216874759
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change Sonnet to depend on tensorflow_probability]


```

---

## 2018-10-11T23:59:24Z -- Change dependency on tf.contrib.distributions to tfp.distributions. (`482fafb4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change dependency on tf.contrib.distributions to tfp.distributions.
PiperOrigin-RevId: 216785589
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change dependency on tf.contrib.distributions to tfp.distributions.]


```

---

## 2018-10-11T17:45:50Z -- Changed inputs.shape to tf.shape(inputs) to allow unknown batch dimension. (`a3f62468`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Changed inputs.shape to tf.shape(inputs) to allow unknown batch dimension.
PiperOrigin-RevId: 216722246
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Changed inputs.shape to tf.shapeinputs to allow unknown batch dimension.]


```

---

## 2018-10-10T16:30:01Z -- Change axis in concat in DeepRNN when using skip_connections. (`ce11b925`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change axis in concat in DeepRNN when using skip_connections.
Previous use cases will work (i.e. cores with shape [batch_size, feature_size] will have the same behaviour), but will enable more sensible concatenation of cores that are multidimensional.

PiperOrigin-RevId: 216543007
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change axis in concat in DeepRNN when using skip_connections.]


```

---

## 2018-10-08T17:29:17Z -- Add `rate` field to the SeparableConv[1,2]D classes. (`058e2915`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add `rate` field to the SeparableConv[1,2]D classes.
PiperOrigin-RevId: 216208732
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add rate field to the SeparableConv[12]D classes.]


```

---

## 2018-10-08T15:13:49Z -- Describe input shape requirements for _ConvND module more accurately. (`ac0d4f51`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Describe input shape requirements for _ConvND module more accurately.
PiperOrigin-RevId: 216188489
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Describe input shape requirements for _ConvND module more accurately.]


```

---

## 2018-10-02T10:06:02Z -- Additional argument-overriding custom getter that only updates defaults, honouring any non-None argument values set in tf.get_variable (or in nested scopes' custom getters). (`ef014251`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Additional argument-overriding custom getter that only updates defaults, honouring any non-None argument values set in tf.get_variable (or in nested scopes' custom getters).
PiperOrigin-RevId: 215360232
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Additional argument-overriding custom getter that only updates defaults honouring any non-None argument values set in tf.get_variable or in nested scopes custom getters.]


```

---

## 2018-10-01T09:16:13Z -- Internal changes. (`fe0874eb`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal changes.
PiperOrigin-RevId: 215180895
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal changes.]


```

---

## 2018-09-28T13:15:00Z -- Add dropout to sonnet's MLP class. (`71065078`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add dropout to sonnet's MLP class.
Dropout is a very useful regularizer that isn't currently supported in sonnet's MLP class. In this CL, we add an argument to the MLP class, `use_dropout`. The `_build` method now takes optional `is_training` and `dropout_keep_probability` arguments.

PiperOrigin-RevId: 214926124
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add dropout to sonnets MLP class.]


```

---

## 2018-09-28T11:56:45Z -- Add Learn to Execute example for Relational Memory Core to sonnet examples. (`f100d0bc`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Learn to Execute example for Relational Memory Core to sonnet examples.
PiperOrigin-RevId: 214918399
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Learn to Execute example for Relational Memory Core to sonnet examples.]


```

---

## 2018-09-20T12:36:12Z -- Adjust test sizes/tags for sanitizers (`c99c3f99`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adjust test sizes/tags for sanitizers
PiperOrigin-RevId: 213795074
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adjust test sizes/tags for sanitizers]


```

---

## 2018-09-19T14:15:41Z -- Adjust test sizes/tags for sanitizers (`1c3d7a8d`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adjust test sizes/tags for sanitizers
PiperOrigin-RevId: 213622590
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adjust test sizes/tags for sanitizers]


```

---

## 2018-09-13T11:12:32Z -- Replace tf.GraphKeys.VARIABLES with tf.GraphKeys.GLOBAL_VARIABLES (`55061cbd`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Replace tf.GraphKeys.VARIABLES with tf.GraphKeys.GLOBAL_VARIABLES
PiperOrigin-RevId: 212790529
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Replace tf.GraphKeys.VARIABLES with tf.GraphKeys.GLOBAL_VARIABLES]


```

---

## 2018-09-11T22:15:20Z -- Fix for n-th farthest task RMC example. Corrects index reference to object. (`e8bd52b7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix for n-th farthest task RMC example. Corrects index reference to object.
PiperOrigin-RevId: 212530863
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix for n-th farthest task RMC example. Corrects index reference to object.]


```

---

## 2018-09-04T12:03:40Z -- Avoid same graph checks in eager mode and stop using `_graph_key`. (`ffa72499`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Avoid same graph checks in eager mode and stop using `_graph_key`.
PiperOrigin-RevId: 211439151
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Avoid same graph checks in eager mode and stop using _graph_key.]


```

---

## 2018-09-03T11:22:22Z -- RNN Shakespeare test: reduce number of training steps from 10 to 5. (`f32ee48c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
RNN Shakespeare test: reduce number of training steps from 10 to 5.
PiperOrigin-RevId: 211337169
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[RNN Shakespeare test: reduce number of training steps from 10 to 5.]


```

---

## 2018-09-01T14:50:23Z -- Fix docstring (`6680867b`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix docstring
PiperOrigin-RevId: 211210870
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix docstring]


```

---

## 2018-08-29T21:04:14Z -- Comment _scale_gradient_op regarding possible memoization requirements. (`5e0234e6`)

**Author:** fviola <fviola@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Comment _scale_gradient_op regarding possible memoization requirements.
PiperOrigin-RevId: 210785875
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Comment _scale_gradient_op regarding possible memoization requirements.]


```

---

## 2018-08-29T15:44:29Z -- Allow Sonnet modules to defun wrap their reuse_variables methods. (`3011932a`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow Sonnet modules to defun wrap their reuse_variables methods.
There was a subtle bug in `_capture_variables` where inside a `defun` we did not
re-enter the Template's variable store (since `executing_eagerly` is False). By
not re-entering the store we break variable re-use (since `get_variable` returns
a new variable instance each time it is called).

PiperOrigin-RevId: 210727568
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow Sonnet modules to defun wrap their reuse_variables methods.]


```

---

## 2018-08-28T13:03:23Z -- Allow Sonnet modules to be defun wrap their call method. (`f18095f7`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow Sonnet modules to be defun wrap their call method.
>>> mlp = snt.nets.MLP([1, 2, 3])
>>> mlp(tf.constant([[1.0]]))
Tensor("mlp_1/linear_2/add:0", shape=(1, 3), dtype=float32)
>>> mlp.defun()
>>> mlp(tf.constant([[1.0]]))
Tensor("PartitionedCall:0", shape=(1, 3), dtype=float32)

By wrapping `_call` and not the whole module we allow properties on the module
to remain accessible (without keeping a reference to the "raw" and defun'd
objects. A good example of this is seen in the updated test.

PiperOrigin-RevId: 210528522
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow Sonnet modules to be defun wrap their call method.]


```

---

## 2018-08-24T13:57:33Z -- Fix docstring typo (`535ccdb2`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix docstring typo
PiperOrigin-RevId: 210092924
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix docstring typo]


```

---

## 2018-08-23T09:14:11Z -- Add clone method to snt.nets.MLP (`a5fcdac8`)

**Author:** arahuja <arahuja@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add clone method to snt.nets.MLP
PiperOrigin-RevId: 209902324
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add clone method to snt.nets.MLP]


```

---

## 2018-08-20T13:08:46Z -- Make `snt.scale_gradient` support eager mode. (`257a62c9`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make `snt.scale_gradient` support eager mode.
Much like optimizers and other tesor taking APIs, in eager mode we require
users to pass us a callable which they want to scale the gradients of.

>>> f = lambda x: tf.pow(x, 2)
>>> f = scale_gradient(f, scale=0.1)
>>> dy_scaled, tfe.gradients_function(f)(x)
>>> print dy_scaled.numpy()  # 0.2

PiperOrigin-RevId: 209405560
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make snt.scale_gradient support eager mode.]


```

---

## 2018-08-17T11:26:11Z -- Implement same graph check using graph keys. (`0e649dad`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Implement same graph check using graph keys.
tl;dr - resolves the last known issues with Sonnet and `tfe.defun`.

Inside a `defun` we observe a different graph instance each time the function is
traced. Sonnet asserts that each time a module is traced that it's graph has not
changed. This CL changes that test to check that the graph we observe each time
has the same "key" (rather than being the same instance). The key is akin to a
primary key for the graph. Graph instances with the same key form part of the
same parent graph (e.g. a sub-graph representing a function with key "a" shares
variables with a regular tf.Graph with key "a" which it is a part of).

DifferentGraphError used to fire undesirably in defun in the following cases
(both of which are fixed in this cl):

1) `enter_variable_scope` is used in conjunction with `_build` (e.g. in the
constructor or via `snt.reuse_variables`.
2) `defun` re-traces `_build` due to the input signature changing (e.g.
Tensor shape changing, or Python parameter values changing).

PiperOrigin-RevId: 209131983
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Implement same graph check using graph keys.]


```

---

## 2018-08-15T08:59:31Z -- Pass optional named arguments to the wrapped sonnet module. (`6a421603`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Pass optional named arguments to the wrapped sonnet module.
Named arguments can be used to change the behavior of sonnet modules. For
example, it's not uncommon to use dropout to regularize a deep neural network.
However, dropout should only be used during the training of the network, not
afterwards.

PiperOrigin-RevId: 208786599
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Pass optional named arguments to the wrapped sonnet module.]


```

---

## 2018-08-14T10:15:37Z -- Make MLP/ConvNet compatible with `tfe.defun`. (`1fb3f4ed`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make MLP/ConvNet compatible with `tfe.defun`.
PiperOrigin-RevId: 208621259
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make MLP/ConvNet compatible with tfe.defun.]


```

---

## 2018-08-09T12:48:53Z -- Make use of `variable_creator_scope` for variable tracking. (`0452bbd6`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make use of `variable_creator_scope` for variable tracking.
Variable creators are stackable factory functions used to control variable
creation. By placing a variable creator at the top of the stack we can observe
all variables being created or re-requested (e.g. via `tf.get_variable`) and
store them in `self._all_variables`.

Additionally this works for the case where a `custom_getter` creates more than
one variable (as in the "Bayes by Backprop" case).

PiperOrigin-RevId: 208034949
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make use of variable_creator_scope for variable tracking.]


```

---

## 2018-08-03T19:33:02Z -- Simplify module stacks by removing weakref to graph. (`1f310fd3`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Simplify module stacks by removing weakref to graph.
We perform same-graph checks agressively when the module is connected, so it
seems to me there is no simple way to end up with multiple-graphs in the module
stack.

The reason I'd like to remove this is that it causes some weirdness with
`defun`, since if two modules `defun` themselves you get two different capturing
graphs which would cause the module stack functionality to break down:

>>> @tfe.defun
... def foo():
...  print 'foo', id(tf.get_default_graph())
...  return bar()

>>> @tfe.defun
... def bar():
...  print 'bar', id(tf.get_default_graph())
...  return tf.ones([])

>>> id(tf.get_default_graph())
140336549583184

>>> foo();
foo 140336571240336
bar 140336555261456

There are other issues with composing defuns which mean it's not simple to add
a Sonnet specific test for this (yet!), however we have many tests which cover
the functionality enabled by the module stack (pushing variables from child to
parent) and those still pass :)

PiperOrigin-RevId: 207307133
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Simplify module stacks by removing weakref to graph.]


```

---

## 2018-08-03T16:14:43Z -- Add count_variables_by_type() to Sonnet. (`34fd32dd`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add count_variables_by_type() to Sonnet.
PiperOrigin-RevId: 207275694
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add count_variables_by_type to Sonnet.]


```

---

## 2018-08-03T12:34:27Z -- Add variable properties to Sonnet modules. (`ebaf4a47`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add variable properties to Sonnet modules.
These methods query `self._all_variables` and don't filter using global
collections. Their name and contract matches many other TF objects (e.g. defun's
`PolymorphicFunction`, `Template` and Keras `Model`).

A nice benefit of this is that you can use defun and access the variables from
your module in a consistent way:

>>> mod = snt.Linear(1)
>>> if FLAGS.use_defun:
...  mod = tfe.defun(mod)
>>> mod(inputs);
>>> mod.variables
(<tf.Variable 'linear/b:0' shape=(1,) dtype=float32_ref>,
<tf.Variable 'linear/w:0' shape=(1, 1) dtype=float32_ref>)

Additionally we follow the more intuitive behaviour of `get_all_variables` so
modules like Sequential don't return the empty collection:

>>> mod = snt.Sequential([snt.Linear(1)])
>>> mod(tf.constant([[1.0]]);
>>> mod.variables
(<tf.Variable 'linear/b:0' shape=(1,) dtype=float32_ref>,
<tf.Variable 'linear/w:0' shape=(1, 1) dtype=float32_ref>)

PiperOrigin-RevId: 207253894
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add variable properties to Sonnet modules.]


```

---

## 2018-07-30T16:32:21Z -- Sonnet version update produced on Tuesday, 31. July 2018 (`f2f8d850`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 31. July 2018
PiperOrigin-RevId: 206595149
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 31. July 2018]


```

---

## 2018-07-26T15:58:57Z -- Add dense/sparse gradient option to the Sonnet Embed module. (`f2f08129`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add dense/sparse gradient option to the Sonnet Embed module.
Adds an option to densify the gradients into a tensor instead of passing indexed-slices. This is beneficial in the backward pass when gradients are passed to the parameter servers, and can speed up performance for moderately sized embeddings.

PiperOrigin-RevId: 206166675
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add dense/sparse gradient option to the Sonnet Embed module.]


```

---

## 2018-07-24T15:02:39Z -- Keep track of last connected output size in DeepRNN. (`dca79d73`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Keep track of last connected output size in DeepRNN.
PiperOrigin-RevId: 205828972
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Keep track of last connected output size in DeepRNN.]


```

---

## 2018-07-17T18:13:55Z -- Compatibility with recent tf variable changes. (`2bf34520`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Compatibility with recent tf variable changes.
PiperOrigin-RevId: 204941059
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Compatibility with recent tf variable changes.]


```

---

## 2018-07-13T10:52:31Z -- Facility to register for notifications of module being connected to the graph. (`44ba888a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Facility to register for notifications of module being connected to the graph.
PiperOrigin-RevId: 204450050
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Facility to register for notifications of module being connected to the graph.]


```

---

## 2018-07-11T11:14:57Z -- Make snt.LSTMBlockCell an actual class for the benefit of code that instantiates using module_name/class_name. (`2b4d757c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make snt.LSTMBlockCell an actual class for the benefit of code that instantiates using module_name/class_name.
PiperOrigin-RevId: 204102372
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make snt.LSTMBlockCell an actual class for the benefit of code that instantiates using module_name/class_name.]


```

---

## 2018-07-10T15:32:05Z -- Internal change (`798e8e09`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 203951281
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2018-07-10T11:24:39Z -- Run Sonnet RNN tests in eager and graph mode. (`f7c759ca`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run Sonnet RNN tests in eager and graph mode.
PiperOrigin-RevId: 203924472
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run Sonnet RNN tests in eager and graph mode.]


```

---

## 2018-07-10T06:43:42Z -- Don't track connected subgraphs in eager mode. (`52129f6b`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Don't track connected subgraphs in eager mode.
In eager mode subgraphs are connected several orders of magnitude more times
than is typical in graph mode. Keeping track of all connected subgraphs means
we hold onto all input/output tensors and end up quite quickly OOM-ing.

PiperOrigin-RevId: 203894783
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Dont track connected subgraphs in eager mode.]


```

---

## 2018-07-06T15:59:23Z -- Run Sonnet base tests in eager and graph. (`5568c56b`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run Sonnet base tests in eager and graph.
PiperOrigin-RevId: 203483937
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run Sonnet base tests in eager and graph.]


```

---

## 2018-07-03T13:13:15Z -- Use semantic_version to check that correct version of TF is installed (`8215c834`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use semantic_version to check that correct version of TF is installed
PiperOrigin-RevId: 203111217
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use semantic_version to check that correct version of TF is installed]


```

---

## 2018-06-29T13:42:59Z -- Expose RNNCellWrapper and wrap_rnn_cell_class (`719cd495`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose RNNCellWrapper and wrap_rnn_cell_class
PiperOrigin-RevId: 202639317
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose RNNCellWrapper and wrap_rnn_cell_class]


```

---

## 2018-06-28T14:45:36Z -- Allow a TensorFlow RNNCell to be wrapped as an RNNCore. (`c2f607c4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow a TensorFlow RNNCell to be wrapped as an RNNCore.
PiperOrigin-RevId: 202477539
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow a TensorFlow RNNCell to be wrapped as an RNNCore.]


```

---

## 2018-06-28T14:07:35Z -- Apply bias only once in ConvLSTM. (`e15eb742`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Apply bias only once in ConvLSTM.
https://github.com/deepmind/sonnet/issues/65

PiperOrigin-RevId: 202473272
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Apply bias only once in ConvLSTM.]


```

---

## 2018-06-28T13:54:39Z -- Extend Conv1DLSTM and Conv2DLSTM to support Layer Norm. (`96721400`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Extend Conv1DLSTM and Conv2DLSTM to support Layer Norm.
PiperOrigin-RevId: 202471664
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Extend Conv1DLSTM and Conv2DLSTM to support Layer Norm.]


```

---

## 2018-06-28T13:41:45Z -- Internal change. (`56b40d69`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 202470337
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2018-06-27T17:53:38Z -- Run Sonnet basic tests in eager and graph mode. (`7a8f6337`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run Sonnet basic tests in eager and graph mode.
PiperOrigin-RevId: 202335246
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run Sonnet basic tests in eager and graph mode.]


```

---

## 2018-06-27T14:49:15Z -- Custom getter that overrides specific named parameters. (`dc7dd5c4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Custom getter that overrides specific named parameters.
PiperOrigin-RevId: 202308055
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Custom getter that overrides specific named parameters.]


```

---

## 2018-06-26T14:24:35Z -- Keep signature of original method when using snt.reuse_variables. (`86147a0c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Keep signature of original method when using snt.reuse_variables.
PiperOrigin-RevId: 202124746
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Keep signature of original method when using snt.reuse_variables.]


```

---

## 2018-06-25T18:13:52Z -- Use `get_shape` instead of `shape` for convolution variables. (`62386f9b`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use `get_shape` instead of `shape` for convolution variables.
If one of convolution's variables is a PartitionedVariable, then it has no `shape` attribute. However, both Variables and PartitionedVariables have a `get_shape()` function. Use this instead. Modify the shared conv tests to catch this.

PiperOrigin-RevId: 201985043
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use get_shape instead of shape for convolution variables.]


```

---

## 2018-06-25T14:18:24Z -- Refactor of VectorQuantizerEMA supporting return of encoding_indices (`bc08dbfb`)

**Author:** fviola <fviola@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Refactor of VectorQuantizerEMA supporting return of encoding_indices
The CL also allows to recover quantized codes from input encoding_indices.

PiperOrigin-RevId: 201949176
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Refactor of VectorQuantizerEMA supporting return of encoding_indices]


```

---

## 2018-06-25T10:36:40Z -- Re-enable Sonnet net tests in eager mode. (`cc8db837`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Re-enable Sonnet net tests in eager mode.
PiperOrigin-RevId: 201929983
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Re-enable Sonnet net tests in eager mode.]


```

---

## 2018-06-21T15:19:20Z -- Internal change. (`11757787`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 201527233
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2018-06-21T12:23:37Z -- Temporarily remove eager/graph annotation until the class level one is released. (`4e5741a4`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Temporarily remove eager/graph annotation until the class level one is released.
PiperOrigin-RevId: 201509633
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Temporarily remove eager/graph annotation until the class level one is released.]


```

---

## 2018-06-13T07:31:24Z -- Add a bidirectional recurrent core to sonnet. (`6926503f`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add a bidirectional recurrent core to sonnet.
Based off encoder component from: https://arxiv.org/pdf/1409.0473.pdf

I avoided tf.while_loop as it complicates the implementation and we want explicit access to states and outputs from all steps of the sequence.

PiperOrigin-RevId: 200346579
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add a bidirectional recurrent core to sonnet.]


```

---

## 2018-06-11T14:01:31Z -- Support for tf.bfloat16 in Sonnet. (`4f34f4fb`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support for tf.bfloat16 in Sonnet.
PiperOrigin-RevId: 200045112
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support for tf.bfloat16 in Sonnet.]


```

---

## 2018-06-08T16:03:33Z -- Add eager mode tests for sonnet nets. (`e61955b5`)

**Author:** tomhennigan <tomhennigan@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add eager mode tests for sonnet nets.
PiperOrigin-RevId: 199800734
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add eager mode tests for sonnet nets.]


```

---

## 2018-06-07T15:26:12Z -- Fix dependencies in examples/BUILD - SciPy was missing. (`ef9146d5`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix dependencies in examples/BUILD - SciPy was missing.
PiperOrigin-RevId: 199638219
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix dependencies in examples/BUILD - SciPy was missing.]


```

---

## 2018-06-07T14:42:56Z -- Make rmc_nth_farthest python3 compatible (`53329387`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make rmc_nth_farthest python3 compatible
PiperOrigin-RevId: 199633253
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make rmc_nth_farthest python3 compatible]


```

---

## 2018-06-07T14:05:08Z -- Fix rmc_nth_farthest.ipynb (`220832e1`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix rmc_nth_farthest.ipynb
PiperOrigin-RevId: 199629407
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix rmc_nth_farthest.ipynb]


```

---

## 2018-06-05T16:52:26Z -- Sonnet version update produced on Tuesday, 05. June 2018 (`31bc79cc`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 05. June 2018
PiperOrigin-RevId: 199312224
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 05. June 2018]


```

---

## 2018-06-05T16:44:04Z -- Updated changelog for version 1.23 (`0c9ed733`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Updated changelog for version 1.23
PiperOrigin-RevId: 199311026
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Updated changelog for version 1.23]


```

---

## 2018-06-04T14:01:06Z -- Fix error message in DeepRNN. (`05e21095`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix error message in DeepRNN.
Because we enumerate through core_sizes[1:] we need to add 1 to i when printing out which core we are referring to in the case of an error. Also fixed an issue where one of the shapes printed had the first dimension removed, and the other one didn't.

PiperOrigin-RevId: 199127178
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix error message in DeepRNN.]


```

---

## 2018-06-04T13:00:31Z -- Change test assertion function. (`3ab2bea8`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change test assertion function.
PiperOrigin-RevId: 199121362
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change test assertion function.]


```

---

## 2018-06-04T12:00:00Z -- Add relational memory module to sonnet. (`5b314d63`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add relational memory module to sonnet.
Adding relational memory implementation from "Relational Recurrent Neural Networks", Santoro et al., 2018.

PiperOrigin-RevId: 199116117
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add relational memory module to sonnet.]


```

---

## 2018-06-01T14:09:19Z -- Sonnet version update produced on Friday, 01. June 2018 (`b66dadcc`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Friday, 01. June 2018
PiperOrigin-RevId: 198867845
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Friday 01. June 2018]


```

---

## 2018-06-01T12:01:17Z -- Switch to using private member variables when possible. (`71c16c11`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Switch to using private member variables when possible.
There can be subtle differences to the public and private versions of member variables in the conv module (eg, Convolution's `stride` is the conv size + 2 but `_stride` is the conv size). The current module mixes and matches these. To reduce cognitive load a bit, we switch to using the private member variable when possible.

PiperOrigin-RevId: 198857690
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Switch to using private member variables when possible.]


```

---

## 2018-05-31T16:50:31Z -- Additional changes to VQVAE notebook to make it python3 compatible. (`3ec0d90d`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Additional changes to VQVAE notebook to make it python3 compatible.
PiperOrigin-RevId: 198735118
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Additional changes to VQVAE notebook to make it python3 compatible.]


```

---

## 2018-05-31T16:06:34Z -- Make brnn_ptb_test write checkpoints to temp directory (`ce869c12`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make brnn_ptb_test write checkpoints to temp directory
PiperOrigin-RevId: 198729764
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make brnn_ptb_test write checkpoints to temp directory]


```

---

## 2018-05-31T11:09:15Z -- MergeDims: also handle dimensions of size zero. (`f6a930bf`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
MergeDims: also handle dimensions of size zero.
PiperOrigin-RevId: 198699387
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[MergeDims: also handle dimensions of size zero.]


```

---

## 2018-05-31T10:13:49Z -- Use six to load cPickle in VQVAE notebook (`6307466d`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use six to load cPickle in VQVAE notebook
PiperOrigin-RevId: 198694898
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use six to load cPickle in VQVAE notebook]


```

---

## 2018-05-30T14:56:05Z -- Updated changelog for Sonnet version 1.21 (`35778ef9`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Updated changelog for Sonnet version 1.21
PiperOrigin-RevId: 198559887
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Updated changelog for Sonnet version 1.21]


```

---

## 2018-05-30T14:54:14Z -- internal change (`d6a02aaf`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
internal change
PiperOrigin-RevId: 198559691
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[internal change]


```

---

## 2018-05-30T14:29:25Z -- Jupyter Notebook demonstrating VQ-VAE training on CIFAR-10. (`45d0f769`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Jupyter Notebook demonstrating VQ-VAE training on CIFAR-10.
PiperOrigin-RevId: 198556907
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Jupyter Notebook demonstrating VQ-VAE training on CIFAR-10.]


```

---

## 2018-05-24T13:42:30Z -- Add control dependencie to VectorQuantizerEMA to avoid potential non-deterministic results. (`024f2632`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add control dependencie to VectorQuantizerEMA to avoid potential non-deterministic results.
PiperOrigin-RevId: 197884089
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add control dependencie to VectorQuantizerEMA to avoid potential non-deterministic results.]


```

---

## 2018-05-24T09:51:21Z -- Improve Sonnet's MergeDim behaviour on partially defined shapes. (`f72c4a53`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve Sonnet's MergeDim behaviour on partially defined shapes.
PiperOrigin-RevId: 197863174
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve Sonnets MergeDim behaviour on partially defined shapes.]


```

---

## 2018-05-22T12:16:54Z -- Add support for custom getters in ConvNet2D (`c3d3bc2b`)

**Author:** sracaniere <sracaniere@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add support for custom getters in ConvNet2D
PiperOrigin-RevId: 197546042
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add support for custom getters in ConvNet2D]


```

---

## 2018-05-21T16:50:47Z -- Add SeparableConv1D class to sonnet. (`8a7ca0ab`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add SeparableConv1D class to sonnet.
PiperOrigin-RevId: 197408513
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add SeparableConv1D class to sonnet.]


```

---

## 2018-05-21T09:41:27Z -- skip_connection deprecation warning is printed only once (`85492dea`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
skip_connection deprecation warning is printed only once
PiperOrigin-RevId: 197370137
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[skip_connection deprecation warning is printed only once]


```

---

## 2018-05-08T15:52:31Z -- Sonnet version update produced on Tuesday, 08. May 2018 (`0277e536`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 08. May 2018
PiperOrigin-RevId: 195825275
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 08. May 2018]


```

---

## 2018-05-04T11:56:10Z -- Add copyright header (`c4ac2b11`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add copyright header
PiperOrigin-RevId: 195399161
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add copyright header]


```

---

## 2018-05-04T11:46:13Z -- Add VQ-VAE plus EMA variant to Sonnet. (`c5950845`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add VQ-VAE plus EMA variant to Sonnet.
PiperOrigin-RevId: 195398448
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add VQ-VAE plus EMA variant to Sonnet.]


```

---

## 2018-04-30T13:54:41Z -- Add snt.summarize_variables to Sonnet. (`f34755cc`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add snt.summarize_variables to Sonnet.
This prints a summary of #variables, #scalars and memory usage for each
datatype.

PiperOrigin-RevId: 194779974
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add snt.summarize_variables to Sonnet.]


```

---

## 2018-04-25T16:50:27Z -- Make an error more explicit when the Sonnet module name is not a string. (`df245c0a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make an error more explicit when the Sonnet module name is not a string.
PiperOrigin-RevId: 194254264
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make an error more explicit when the Sonnet module name is not a string.]


```

---

## 2018-04-24T16:26:28Z -- Sonnet version update produced on Tuesday, 24. April 2018 (`c838ebfc`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 24. April 2018
PiperOrigin-RevId: 194098244
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 24. April 2018]


```

---

## 2018-04-24T15:36:24Z -- Make brnn_ptb and ptb_reader python3 compatible. (`6db202c7`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make brnn_ptb and ptb_reader python3 compatible.
Fixes https://github.com/deepmind/sonnet/issues/79

PiperOrigin-RevId: 194091717
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make brnn_ptb and ptb_reader python3 compatible.]


```

---

## 2018-04-24T14:40:05Z -- Update installation instructions (`50b41df0`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update installation instructions
PiperOrigin-RevId: 194085445
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update installation instructions]


```

---

## 2018-04-24T13:45:10Z -- Add a unit test for brnn_ptb. (`a30da967`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add a unit test for brnn_ptb.
Runs a small model with fake data for 1 epoch.

PiperOrigin-RevId: 194079346
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add a unit test for brnn_ptb.]


```

---

## 2018-04-16T09:31:21Z -- Merge SeparableConv2D into _ConvND (`5c7e3559`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge SeparableConv2D into _ConvND
Merge the final convolution class into _ConvND. We also add a property field for `channel_multiplier` in `DepthwiseConv2D` as well as `SeparableConv2D`.

PiperOrigin-RevId: 193007472
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge SeparableConv2D into _ConvND]


```

---

## 2018-04-10T16:00:00Z -- Sonnet version update produced on Tuesday, 10. April 2018 (`1a313f6a`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Tuesday, 10. April 2018
PiperOrigin-RevId: 192293232
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Tuesday 10. April 2018]


```

---

## 2018-04-09T15:35:19Z -- Remove skip_connnection option from ConvLSTM. (`a86f0445`)

**Author:** fviola <fviola@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove skip_connnection option from ConvLSTM.
PiperOrigin-RevId: 192131304
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove skip_connnection option from ConvLSTM.]


```

---

## 2018-04-09T15:17:07Z -- Update test (`600268f8`)

**Author:** fviola <fviola@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update test
PiperOrigin-RevId: 192129236
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update test]


```

---

## 2018-04-09T13:13:47Z -- Move CausalConv related functionality into subclass. (`5980cf90`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move CausalConv related functionality into subclass.
There are a few bits of functionality that are only related to CausalConv1D and don't need to be in _ConvND. We're moving them into the subclass and calling _ConvND machinery for the rest.

PiperOrigin-RevId: 192117666
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move CausalConv related functionality into subclass.]


```

---

## 2018-04-09T13:03:43Z -- Refactor DepthwiseConv2D to use _ConvND. (`1ca68669`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Refactor DepthwiseConv2D to use _ConvND.
Rewrite DepthwiseConv2D so that it shares as much functionality with _ConvND as possible.

PiperOrigin-RevId: 192116657
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Refactor DepthwiseConv2D to use _ConvND.]


```

---

## 2018-04-09T11:22:14Z -- Refactor InPlaneConv2D to use _ConvND (`23cb8d93`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Refactor InPlaneConv2D to use _ConvND
This refactor removes almost all of the code in InPlaneConv2D and uses _ConvND instead.

We also fix some tests that were failing when run on the GPU in conv_gpu_test due to numpy & tensorflow version upgrades.

PiperOrigin-RevId: 192109587
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Refactor InPlaneConv2D to use _ConvND]


```

---

## 2018-04-06T15:49:01Z -- Add dilation rate tests for ConvNet2D. (`98027141`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add dilation rate tests for ConvNet2D.
Adding testing for the `rates` argument in ConvNet2D.

PiperOrigin-RevId: 191893167
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add dilation rate tests for ConvNet2D.]


```

---

## 2018-03-26T12:21:05Z -- Remove spurious spaces in doc. (`159adc55`)

**Author:** sracaniere <sracaniere@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove spurious spaces in doc.
PiperOrigin-RevId: 190449915
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove spurious spaces in doc.]


```

---

## 2018-03-13T14:47:10Z -- Use tensorflow nest for nest operations. (`e6609310`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use tensorflow nest for nest operations.
PiperOrigin-RevId: 188871826
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use tensorflow nest for nest operations.]


```

---

## 2018-03-13T13:11:16Z -- Use nest from pytflib to get rid of deprecation messages. (`1bee5931`)

**Author:** sracaniere <sracaniere@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use nest from pytflib to get rid of deprecation messages.
PiperOrigin-RevId: 188862864
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use nest from pytflib to get rid of deprecation messages.]


```

---

## 2018-03-13T12:49:01Z -- Sonnet version update produced on Monday, 12. March 2018 (`4596fd04`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 12. March 2018
PiperOrigin-RevId: 188861040
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 12. March 2018]


```

---

## 2018-03-09T13:53:39Z -- Fix typo in DeepRNN docs. (`80d47843`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix typo in DeepRNN docs.
PiperOrigin-RevId: 188474053
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix typo in DeepRNN docs.]


```

---

## 2018-03-07T13:30:37Z -- Improve the way reuse_variables handles name scopes (`7d65b8a9`)

**Author:** gabrielbm <gabrielbm@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve the way reuse_variables handles name scopes
This fixes a bug where outer name scopes were ignored by reuse_variables.

PiperOrigin-RevId: 188163153
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve the way reuse_variables handles name scopes]


```

---

## 2018-03-05T17:55:14Z -- Docstring clarification (`b1191ad3`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Docstring clarification
PiperOrigin-RevId: 187880593
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Docstring clarification]


```

---

## 2018-03-01T11:49:03Z -- Add mention to get_all_variables in snt.Sequential warning. (`e62f660e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add mention to get_all_variables in snt.Sequential warning.
PiperOrigin-RevId: 187456723
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add mention to get_all_variables in snt.Sequential warning.]


```

---

## 2018-02-23T17:35:53Z -- Added optional dilation rates argument for ConvNet2D. (`553159b4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added optional dilation rates argument for ConvNet2D.
PiperOrigin-RevId: 186779967
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added optional dilation rates argument for ConvNet2D.]


```

---

## 2018-02-23T12:53:10Z -- Sort variables returned by get_all_variables() by name. (`c8556727`)

**Author:** gabrielbm <gabrielbm@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sort variables returned by get_all_variables() by name.
The variables returned by get_all_variables() are stored in a set, so they must be sorted to determine an ordering over them. This change sorts the variables by name before returning them.

PiperOrigin-RevId: 186752578
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sort variables returned by get_all_variables by name.]


```

---

## 2018-02-22T15:16:07Z -- Add missing argument to example code. (`a1d1a374`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add missing argument to example code.
PiperOrigin-RevId: 186613534
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add missing argument to example code.]


```

---

## 2018-02-21T16:32:06Z -- Bump version to 1.17 (`108fce7b`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump version to 1.17
PiperOrigin-RevId: 186463459
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump version to 1.17]


```

---

## 2018-02-14T11:58:02Z -- Updated README - TensorFlow v1.5 required (`c522ae68`)

**Author:** Aditya Paliwal <adityapaliwal@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Updated README - TensorFlow v1.5 required
GIT_ORIGIN_REV_ID=264bc35ad75f1dad605407295e857846237917be
PiperOrigin-RevId: 185666441
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Updated README - TensorFlow v1.5 required]


```

---

## 2018-02-14T11:38:14Z -- Implementation of get_all_variables() for sonnet modules. (`62f43990`)

**Author:** gabrielbm <gabrielbm@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Implementation of get_all_variables() for sonnet modules.
We introduce a module call stack, which tracks the order in which modules are called. When a module enters __call__ (or _enter_variable_scope) it adds itself to the top of the stack. Variables created inside of the custom_getter are added to a collection specific to the module on the top of the stack. Before exiting __call__ (or _enter_variable_scope) the module moves all variables added to this graph collection into `_all_variables`, removes itself from the top of the stack, and adds all of the variables from `self._all_variables` to collection for the module that is currently at the top of the module stack.

PiperOrigin-RevId: 185664981
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Implementation of get_all_variables for sonnet modules.]


```

---

## 2018-02-13T14:50:57Z -- Replace keep_dims with keepdims in call to tf.reduce_prod() (`06b8e4f9`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Replace keep_dims with keepdims in call to tf.reduce_prod()
PiperOrigin-RevId: 185524194
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Replace keep_dims with keepdims in call to tf.reduce_prod]


```

---

## 2018-02-13T11:50:15Z -- Fix an error message which was incorrect. (`f9e38596`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix an error message which was incorrect.
PiperOrigin-RevId: 185509698
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix an error message which was incorrect.]


```

---

## 2018-01-30T15:55:29Z -- Fix Python3 incompatibilities in new tests and methods. (`551d6528`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix Python3 incompatibilities in new tests and methods.
PiperOrigin-RevId: 183831798
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix Python3 incompatibilities in new tests and methods.]


```

---

## 2018-01-29T17:55:18Z -- Add backwards compatibility with tests for ones and zeros name scopes. (`2136d318`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add backwards compatibility with tests for ones and zeros name scopes.
PiperOrigin-RevId: 183680464
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add backwards compatibility with tests for ones and zeros name scopes.]


```

---

## 2018-01-29T11:57:14Z -- Remove fixed seed dependency in gated_rnn_test.LSTMTest.testRecurrentDropout (`1d2d99e5`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove fixed seed dependency in gated_rnn_test.LSTMTest.testRecurrentDropout
PiperOrigin-RevId: 183644341
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove fixed seed dependency in gated_rnn_test.LSTMTest.testRecurrentDropout]


```

---

## 2018-01-25T16:44:55Z -- Enable custom_getter for TrainableVariable. (`84bab04b`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Enable custom_getter for TrainableVariable.
PiperOrigin-RevId: 183243768
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Enable custom_getter for TrainableVariable.]


```

---

## 2018-01-25T14:33:20Z -- Refactor out inputs verification. (`438b320e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Refactor out inputs verification.
Remove duplicate code that is performing the same checks on the `inputs` tensor in every Convolution class.

PiperOrigin-RevId: 183230711
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Refactor out inputs verification.]


```

---

## 2018-01-24T13:09:04Z -- Fix typo. (`2a8e6e9f`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix typo.
PiperOrigin-RevId: 183071198
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix typo.]


```

---

## 2018-01-22T11:46:16Z -- Correct typo in warning. (`3aead325`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Correct typo in warning.
PiperOrigin-RevId: 182755077
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Correct typo in warning.]


```

---

## 2018-01-19T12:11:15Z -- Make masked Conv2D usable with ResourceVariables by avoiding use of *=. (`109d6060`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make masked Conv2D usable with ResourceVariables by avoiding use of *=.
PiperOrigin-RevId: 182516172
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make masked Conv2D usable with ResourceVariables by avoiding use of .]


```

---

## 2018-01-16T14:13:07Z -- Explicitly enter the scope of the connected Graph in AbstractModule.get_variables() (`7393cfbb`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Explicitly enter the scope of the connected Graph in AbstractModule.get_variables()
This fixeds the following inconsistency:
```
with tf.Graph().as_default() as graph:
input = tf.constant(np.random.randn(16, 784))
lin = snt.Linear(output_size=256)
output = lin(input)
print(lin.get_variables())  # prints tuple of 2 variables
print(lin.get_variables())  # prints empty tuple, as technically we are in a different Graph.
```

Also added a .graph readonly property to AbstractModule.

PiperOrigin-RevId: 182044744
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Explicitly enter the scope of the connected Graph in AbstractModule.get_variables]


```

---

## 2018-01-15T20:38:19Z -- Provide better examples for snt.reuse_variables. (`7bdd40c0`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Provide better examples for snt.reuse_variables.
PiperOrigin-RevId: 181984575
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Provide better examples for snt.reuse_variables.]


```

---

## 2018-01-12T15:51:09Z -- Push BBB library and Bayesian RNN example to open source sonnet. (`3943452a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Push BBB library and Bayesian RNN example to open source sonnet.
PiperOrigin-RevId: 181743627
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Push BBB library and Bayesian RNN example to open source sonnet.]


```

---

## 2018-01-12T11:35:07Z -- Select the correct input channel and stride values within Conv{1,2,3}DTranspose.transpose() when the data_format is NC*. (`315719ef`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Select the correct input channel and stride values within Conv{1,2,3}DTranspose.transpose() when the data_format is NC*.
Add more tests for Conv*Transpose transpose functionality.

PiperOrigin-RevId: 181724912
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Select the correct input channel and stride values within Conv123DTranspose.transpose when the data_format is NC.]


```

---

## 2018-01-11T15:08:07Z -- Improve an error message. (`7865f86e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve an error message.
This is supposing `data_format` is a string, but if one is incorrectly passing something else, the error message will fail e.g. with ValueError: Unknown format code 's' for object of type 'int'

With this change the error becomes:
ValueError: Invalid data_format 256. Allowed formats set(['NCHW', 'NHWC'])
PiperOrigin-RevId: 181604070
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve an error message.]


```

---

## 2018-01-11T11:56:09Z -- Remove dependency on fixed seed for testZoneout. (`3a21b8d0`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove dependency on fixed seed for testZoneout.
This means it won't be possible to test for expected state values.

PiperOrigin-RevId: 181587515
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove dependency on fixed seed for testZoneout.]


```

---

## 2018-01-09T11:04:00Z -- Add Con[1-3]DTranspose classes to conv_gpu_tests. (`f11b5af6`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Con[1-3]DTranspose classes to conv_gpu_tests.
Expanding testing coverage of the transpose classes.

PiperOrigin-RevId: 181293937
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Con[1-3]DTranspose classes to conv_gpu_tests.]


```

---

## 2018-01-05T19:42:45Z -- Add Causal1DConv testing to conv_gpu_test. (`3c18209e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Causal1DConv testing to conv_gpu_test.
PiperOrigin-RevId: 180955048
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Causal1DConv testing to conv_gpu_test.]


```

---

## 2018-01-05T18:04:43Z -- Stop using constant initializers in conv_gpu_test.py. (`a1b1f343`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Stop using constant initializers in conv_gpu_test.py.
Unit tests were using constant initializers to check for correctness in data_format permutation operations. This is problematic; you want the weights to be very random so that they don't inadvertently mask an issue in their functionality.

PiperOrigin-RevId: 180941577
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Stop using constant initializers in conv_gpu_test.py.]


```

---

## 2018-01-05T13:24:42Z -- Rename SUPPORTED_DATA_FORMATS to SUPPORTED_2D_DATA_FORMATS. (`fe910806`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Rename SUPPORTED_DATA_FORMATS to SUPPORTED_2D_DATA_FORMATS.
As we support far more than just 2D conv now, this variable needs to change.

PiperOrigin-RevId: 180915317
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Rename SUPPORTED_DATA_FORMATS to SUPPORTED_2D_DATA_FORMATS.]


```

---

## 2018-01-05T12:41:21Z -- Refactor CausalConv1D to use _ConvND. Tests for clone functionality in all _ConvND subclasses. (`865f4aa9`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Refactor CausalConv1D to use _ConvND. Tests for clone functionality in all _ConvND subclasses.
CausalConv1D can exist using the new _ConvND superclass. Add support within _ConvND to accommodate padded input. Refactored _ConvND a bit to get smaller methods as _build() was getting unwieldy.

With CausalConv1D inheriting from _ConvND, it is now cloneable by default. I noticed that most of the classes lacked testing of their clone functionality, so I added those.

Finally, in supporting multiple data_formats, I noticed there aren't enough tests that check to see an exception is thrown when an invalid data_format is used. So I added those.

PiperOrigin-RevId: 180912740
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Refactor CausalConv1D to use _ConvND. Tests for clone functionality in all _ConvND subclasses.]


```

---

## 2018-01-05T11:12:33Z -- Refactor Conv{1,2,3}Transpose classes into one. Add tests for transpose functionality. (`699f8cb5`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Refactor Conv{1,2,3}Transpose classes into one. Add tests for transpose functionality.
Rearchitect the Transpose classes into one base class that is then instantiated for each dimension convolution we want. Add tests for Conv{1,3}Transpose; we make sure to test N*C and NC* data formats.

PiperOrigin-RevId: 180907942
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Refactor Conv123Transpose classes into one. Add tests for transpose functionality.]


```

---

## 2018-01-03T20:47:11Z -- Add a Recurrent Highway Network cell. (`511f11b1`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add a Recurrent Highway Network cell.
PiperOrigin-RevId: 180705745
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add a Recurrent Highway Network cell.]


```

---

## 2018-01-03T16:40:55Z -- Improve snt.Embed performance in distributed training. (`5fe263b9`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve snt.Embed performance in distributed training.
Adds a fix to avoid excess computation on parameter servers.

PiperOrigin-RevId: 180674688
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve snt.Embed performance in distributed training.]


```

---

## 2018-01-02T11:14:13Z -- Update AbstractModule documentation. (`6f9f46ec`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update AbstractModule documentation.
PiperOrigin-RevId: 180531448
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update AbstractModule documentation.]


```

---

## 2017-12-22T13:32:52Z -- Creation of a ConvND class. More flexible masks. Lots more tests. (`ca13720e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Creation of a ConvND class. More flexible masks. Lots more tests.
Creating a ConvND class that is then subclassed for Conv{1,2,3}D. This removes a lot of duplicated boilerplate code. As part of this we, are also adding a `mask` argument to the Conv{1,3}D classes. Finally, we remove rank restrictions on the mask argument. We add testing for new masking functionality.

PiperOrigin-RevId: 179918026
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Creation of a ConvND class. More flexible masks. Lots more tests.]


```

---

## 2017-12-22T12:47:04Z -- Extend batch_norm_v2 data_format default across dimensions. (`d0f50a65`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Extend batch_norm_v2 data_format default across dimensions.
PiperOrigin-RevId: 179915706
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Extend batch_norm_v2 data_format default across dimensions.]


```

---

## 2017-12-22T11:34:10Z -- Improve snt.Embed performance in distributed training. (`89c2091b`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve snt.Embed performance in distributed training.
Adds a fix to avoid excess computation on parameter servers.

PiperOrigin-RevId: 179912285
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve snt.Embed performance in distributed training.]


```

---

## 2017-12-21T16:56:19Z -- Implemented snt.BatchNormV2, which differs from snt.BatchNorm in the following ways: (`9a547ed7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Implemented snt.BatchNormV2, which differs from snt.BatchNorm in the following ways:
* Automatically computes updates to moving statistics by default (i.e. update_ops_collection=None).
* Uses moving statistics by default when testing (i.e. test_local_stats=False).
* Takes a data_format string (NC/NWC/NCW/NHWC/NCHW/NDHWC/NCDHW) rather than axes; reduces along all non-C axes.
* Uses fused batch normalization by default. If the data_format isn't NHWC or NCHW, reshapes the batch internally.
* Uses flat variables for the moving statistics, scale, and offset so that they can be shared between different data_formats.

PiperOrigin-RevId: 179819339
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Implemented snt.BatchNormV2 which differs from snt.BatchNorm in the following ways:]


```

---

## 2017-12-20T19:52:49Z -- tf.float16 support for batch_norm (`b9205708`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
tf.float16 support for batch_norm
PiperOrigin-RevId: 179714930
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[tf.float16 support for batch_norm]


```

---

## 2017-12-19T17:49:09Z -- Improve error message when partitioners/regularizers/initializers used wrong. (`e2ce831b`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve error message when partitioners/regularizers/initializers used wrong.
PiperOrigin-RevId: 179566444
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve error message when partitioners/regularizers/initializers used wrong.]


```

---

## 2017-12-19T17:48:00Z -- When Sequential contains no layers, simply act as identity. (`3b9f6362`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
When Sequential contains no layers, simply act as identity.
Previously, a sequential with no layers would return any input wrapped in a 1
element tuple, which is a strange inconsistency.

PiperOrigin-RevId: 179566305
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[When Sequential contains no layers simply act as identity.]


```

---

## 2017-12-19T17:10:36Z -- Improve error message when partitioners/regularizers/initializers used wrong. (`a7a557e6`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve error message when partitioners/regularizers/initializers used wrong.
PiperOrigin-RevId: 179561994
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve error message when partitioners/regularizers/initializers used wrong.]


```

---

## 2017-12-19T16:14:37Z -- Corrected parameter list for testFusedBatchNorm, where `is_training` had been erroneously set to `True` in some cases. (`f2d6f4b7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Corrected parameter list for testFusedBatchNorm, where `is_training` had been erroneously set to `True` in some cases.
PiperOrigin-RevId: 179555601
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Corrected parameter list for testFusedBatchNorm where is_training had been erroneously set to True in some cases.]


```

---

## 2017-12-19T13:25:05Z -- Improve error message when partitioners/regularizers/initializers used wrong. (`293e86de`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve error message when partitioners/regularizers/initializers used wrong.
PiperOrigin-RevId: 179541337
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve error message when partitioners/regularizers/initializers used wrong.]


```

---

## 2017-12-18T11:56:13Z -- Support tf.float16 inputs in convolution modules. (`13c724a1`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support tf.float16 inputs in convolution modules.
PiperOrigin-RevId: 179402639
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support tf.float16 inputs in convolution modules.]


```

---

## 2017-12-18T10:52:11Z -- Conv3DTranspose to initialize biases to zero by default. (`26013e4f`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Conv3DTranspose to initialize biases to zero by default.
PiperOrigin-RevId: 179398434
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Conv3DTranspose to initialize biases to zero by default.]


```

---

## 2017-12-18T09:53:03Z -- Adjust test to use .assertDictEqual() rather than .assertItemsEqual() on dicts. (`85e1b7ac`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adjust test to use .assertDictEqual() rather than .assertItemsEqual() on dicts.
Reason: Latter comparison, when used on dicts, only compares dict-keys, ignoring
dict-values.
PiperOrigin-RevId: 179394091
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adjust test to use .assertDictEqual rather than .assertItemsEqual on dicts.]


```

---

## 2017-12-15T20:31:48Z -- Get gated_rnn_test passing again (`beb3fb5c`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Get gated_rnn_test passing again
PiperOrigin-RevId: 179226394
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Get gated_rnn_test passing again]


```

---

## 2017-12-12T14:05:16Z -- Change "deprecated" to "not supported". (`ebd18c8d`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change "deprecated" to "not supported".
PiperOrigin-RevId: 178751493
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change deprecated to not supported.]


```

---

## 2017-12-05T17:12:58Z -- Use a namedtuple for LSTM state, so can safely access the cell and hidden components. (`a1547dd9`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use a namedtuple for LSTM state, so can safely access the cell and hidden components.
PiperOrigin-RevId: 177963672
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use a namedtuple for LSTM state so can safely access the cell and hidden components.]


```

---

## 2017-11-30T15:45:46Z -- Slice TensorShape instead of Tensor in basic_rnn.py. (`bfe2962a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Slice TensorShape instead of Tensor in basic_rnn.py.
This prevents DeepRNN.output_size() from creating a slice operation on
a Tensor declared inside the dynamic RNN loop body (output_size() is
called outside the loop body, so the resulting slice operation is
invalid).  This currently works because the slice operation isn't
actually run (since it's only used to get its shape), but in the
future creating operations with invalid inputs will raise an
exception.

PiperOrigin-RevId: 177453503
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Slice TensorShape instead of Tensor in basic_rnn.py.]


```

---

## 2017-11-27T16:25:48Z -- Adds NCHW support to Conv1D and CausalConv1D. (`1ff69071`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adds NCHW support to Conv1D and CausalConv1D.
PiperOrigin-RevId: 177019383
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adds NCHW support to Conv1D and CausalConv1D.]


```

---

## 2017-11-23T11:28:59Z -- Add a zoneout wrapper for recurrent neural networks and specialize it for LSTM. (`df4b83df`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add a zoneout wrapper for recurrent neural networks and specialize it for LSTM.
PiperOrigin-RevId: 176755442
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add a zoneout wrapper for recurrent neural networks and specialize it for LSTM.]


```

---

## 2017-11-21T17:49:47Z -- Add space at end of deprecation warning. (`6f95f75c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add space at end of deprecation warning.
PiperOrigin-RevId: 176521653
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add space at end of deprecation warning.]


```

---

## 2017-11-21T14:59:43Z -- Add a recurrent dropout wrapper and specialize it for LSTM. (`d8503c89`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add a recurrent dropout wrapper and specialize it for LSTM.
PiperOrigin-RevId: 176502360
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add a recurrent dropout wrapper and specialize it for LSTM.]


```

---

## 2017-11-21T10:59:52Z -- Change deprecation update message and nest import (`96d26661`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change deprecation update message and nest import
PiperOrigin-RevId: 176485081
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change deprecation update message and nest import]


```

---

## 2017-11-20T15:52:01Z -- Sonnet LSTM: add optional projection of hidden recurrent state. (`90c37e0d`)

**Author:** jwrae <jwrae@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet LSTM: add optional projection of hidden recurrent state.
PiperOrigin-RevId: 176359570
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet LSTM: add optional projection of hidden recurrent state.]


```

---

## 2017-11-20T15:33:47Z -- Sonnet version update produced on Monday, 20. November 2017 (`52edfd08`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 20. November 2017
PiperOrigin-RevId: 176357249
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 20. November 2017]


```

---

## 2017-11-20T11:51:29Z -- Update batch-norm example in comment. (`6a13bca8`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update batch-norm example in comment.
PiperOrigin-RevId: 176338001
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update batch-norm example in comment.]


```

---

## 2017-11-16T17:01:04Z -- Replace sonnet.nest *iterable functions with their equivalent from TF. (`eeccbfba`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Replace sonnet.nest *iterable functions with their equivalent from TF.
PiperOrigin-RevId: 175970632
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Replace sonnet.nest iterable functions with their equivalent from TF.]


```

---

## 2017-11-14T16:10:07Z -- Add custom_getter option to snt.Embed. (`6faf4853`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add custom_getter option to snt.Embed.
PiperOrigin-RevId: 175680730
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add custom_getter option to snt.Embed.]


```

---

## 2017-11-14T14:39:47Z -- Expose util.custom_getter_router, which was private. (`71f179db`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose util.custom_getter_router, which was private.
PiperOrigin-RevId: 175672207
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose util.custom_getter_router which was private.]


```

---

## 2017-11-09T16:00:49Z -- Fix :base build rule dependency on :module_pb2. (`a6cb7f51`)

**Author:** Diego de Las Casas <diegodelascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix :base build rule dependency on :module_pb2.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix :base build rule dependency on :module_pb2.]


```

---

## 2017-11-09T15:58:31Z -- Revert initial implementation for eager mode. (`dcf8c90c`)

**Author:** Diego de Las Casas <diegodelascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Revert initial implementation for eager mode.
Reverted due to incompatibility with Tensorflow 1.4.0.

This reverts commit e1ef6d503205a7d9790ae45f3c77457992c88e03.
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Revert initial implementation for eager mode.]


```

---

## 2017-11-09T15:07:22Z -- Internal change. (`237c385c`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 175153104
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2017-11-09T14:38:28Z -- Clarify TensorFlow version (`0a2c31fc`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Clarify TensorFlow version
PiperOrigin-RevId: 175150554
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Clarify TensorFlow version]


```

---

## 2017-11-09T14:36:17Z -- Add copyright header to module.proto (`b30a0e56`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add copyright header to module.proto
PiperOrigin-RevId: 175150376
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add copyright header to module.proto]


```

---

## 2017-11-09T14:31:35Z -- Update Changelog (`7e6193d7`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update Changelog
PiperOrigin-RevId: 175150004
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update Changelog]


```

---

## 2017-11-09T13:15:09Z -- Correct link in CONTRIBUTING.md (`1e580608`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Correct link in CONTRIBUTING.md
PiperOrigin-RevId: 175144245
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Correct link in CONTRIBUTING.md]


```

---

## 2017-11-09T12:38:16Z -- Add version information in the init. (`5f966b5d`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add version information in the init.
PiperOrigin-RevId: 175141587
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add version information in the init.]


```

---

## 2017-11-03T18:01:59Z -- Allowing __iter__ over 1+dimensional tensors with known shapes. (`9d7b9fbb`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allowing __iter__ over 1+dimensional tensors with known shapes.
PiperOrigin-RevId: 174484601
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allowing __iter__ over 1dimensional tensors with known shapes.]


```

---

## 2017-11-03T16:39:42Z -- Removal of deprecated sonnet/testing/parameterized module due to its migration to Abseil. (`5c44640e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Removal of deprecated sonnet/testing/parameterized module due to its migration to Abseil.
PiperOrigin-RevId: 174472509
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Removal of deprecated sonnet/testing/parameterized module due to its migration to Abseil.]


```

---

## 2017-11-02T17:30:21Z -- Internal changes. (`0356d005`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal changes.
PiperOrigin-RevId: 174345289
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal changes.]


```

---

## 2017-11-02T13:11:30Z -- snt.BatchApply now passes scalar-valued non-tensor inputs, such as Boolean flags, without converting them to tensors. (`354bf3fd`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
snt.BatchApply now passes scalar-valued non-tensor inputs, such as Boolean flags, without converting them to tensors.
PiperOrigin-RevId: 174316427
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[snt.BatchApply now passes scalar-valued non-tensor inputs such as Boolean flags without converting them to tensors.]


```

---

## 2017-10-31T19:53:46Z -- Adds NCHW support to DepthwiseConv2D and SeparableConv2D. (`dc09af16`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adds NCHW support to DepthwiseConv2D and SeparableConv2D.
Fixes weight initialisation scaling of DepthwiseConv2D and SeparableConv2D.

PiperOrigin-RevId: 174077490
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adds NCHW support to DepthwiseConv2D and SeparableConv2D.]


```

---

## 2017-10-31T17:57:24Z -- In BatchApply, when merged batch dimension can be inferred, then do so. (`045c8736`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
In BatchApply, when merged batch dimension can be inferred, then do so.
PiperOrigin-RevId: 174059838
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[In BatchApply when merged batch dimension can be inferred then do so.]


```

---

## 2017-10-30T18:23:18Z -- Migrate to `absl` open-source library. (`7517d2e7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Migrate to `absl` open-source library.
PiperOrigin-RevId: 173921337
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Migrate to absl open-source library.]


```

---

## 2017-10-30T12:34:53Z -- Make merge_leading_dims and split_leading_dim part of the public sonnet API. (`87c15720`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make merge_leading_dims and split_leading_dim part of the public sonnet API.
PiperOrigin-RevId: 173879037
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make merge_leading_dims and split_leading_dim part of the public sonnet API.]


```

---

## 2017-10-30T12:14:54Z -- Make mask argument to Conv2D more flexible (`869b5582`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make mask argument to Conv2D more flexible
The mask argument to Conv2D needed to be something that is convertible to a
numpy array. The numpy mask was then automatically converted to a TensorFlow
constant when applied.
Because the constants are stored in the graph, this increases the size of graph
quite considerably (by the size of the Conv2D filters).
If we allow the user to pass in a TensorFlow Tensor instead, it is easily
possible to use a PyFunc wrapper around a numpy array, or to cache constants
in between calls to Conv2D.
This extends Conv2D to be able to handle any mask input that is convertible
to a float32 or float64 Tensor.

PiperOrigin-RevId: 173877530
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make mask argument to Conv2D more flexible]


```

---

## 2017-10-30T11:59:02Z -- Internal change. (`4245e51c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 173876219
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---

## 2017-10-30T10:21:53Z -- Cast input shape to tuple to allow for robust concatenation of shape information (`20a32c7a`)

**Author:** fviola <fviola@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Cast input shape to tuple to allow for robust concatenation of shape information
PiperOrigin-RevId: 173870205
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Cast input shape to tuple to allow for robust concatenation of shape information]


```

---

## 2017-10-12T18:50:49Z -- Update docstring for snt.custom_getters. (`469a9330`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update docstring for snt.custom_getters.
PiperOrigin-RevId: 171990392
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update docstring for snt.custom_getters.]


```

---

## 2017-10-10T14:39:29Z -- Fixed typo in doc string. (`d80a554e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed typo in doc string.
PiperOrigin-RevId: 171676263
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed typo in doc string.]


```

---

## 2017-10-09T16:20:51Z -- Make sure module `is_connected` if it's connected to graph using a `@reuse_variables` method other than `_build`. (`da324864`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make sure module `is_connected` if it's connected to graph using a `@reuse_variables` method other than `_build`.
PiperOrigin-RevId: 171543901
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make sure module is_connected if its connected to graph using a reuse_variables method other than _build.]


```

---

## 2017-10-09T14:17:55Z -- Sonnet version update produced on Monday, 09. October 2017 (`b5bc2e53`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 09. October 2017
PiperOrigin-RevId: 171531910
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 09. October 2017]


```

---

## 2017-10-09T11:13:01Z -- Avoid recursion when serializing ModuleInfo. (`39998680`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Avoid recursion when serializing ModuleInfo.
PiperOrigin-RevId: 171519454
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Avoid recursion when serializing ModuleInfo.]


```

---

## 2017-10-06T17:07:45Z -- scale_gradient now handles all float dtypes. (`365ddaa2`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
scale_gradient now handles all float dtypes.
PiperOrigin-RevId: 171306048
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[scale_gradient now handles all float dtypes.]


```

---

## 2017-10-06T15:00:00Z -- Defun in clip_gradient takes tensor min/max clip value args in forward pass so that they can be used in backward pass. (`83603ce5`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Defun in clip_gradient takes tensor min/max clip value args in forward pass so that they can be used in backward pass.
PiperOrigin-RevId: 171292623
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Defun in clip_gradient takes tensor min/max clip value args in forward pass so that they can be used in backward pass.]


```

---

## 2017-10-05T16:28:47Z -- Added `class_name` to ModuleInfo. (`74ef80ea`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added `class_name` to ModuleInfo.
PiperOrigin-RevId: 171163066
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added class_name to ModuleInfo.]


```

---

## 2017-10-05T11:16:41Z -- Decorating the _build function with memoize is breaking the connected_subgraph code. This CL fixes this issue. Note sure if this use-case is valid in the first place but let's have this discussion later! (`84f4c4e7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Decorating the _build function with memoize is breaking the connected_subgraph code. This CL fixes this issue. Note sure if this use-case is valid in the first place but let's have this discussion later!
PiperOrigin-RevId: 171134926
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Decorating the _build function with memoize is breaking the connected_subgraph code. This CL fixes this issue. Note sure if this use-case is valid in the first place but lets have this discussion later]


```

---

## 2017-09-28T23:25:42Z -- Allow ConvNet to use NCHW data_format (`f5633caf`)

**Author:** arahuja <arahuja@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow ConvNet to use NCHW data_format
PiperOrigin-RevId: 170415564
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow ConvNet to use NCHW data_format]


```

---

## 2017-09-27T12:02:27Z -- Fix stride property on Conv2D for NCHW inputs. (`9743f766`)

**Author:** arahuja <arahuja@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix stride property on Conv2D for NCHW inputs.
PiperOrigin-RevId: 170182706
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix stride property on Conv2D for NCHW inputs.]


```

---

## 2017-09-25T18:18:57Z -- Clean up and improve example text for snt.custom_getters.Context. (`5d31e36e`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Clean up and improve example text for snt.custom_getters.Context.
Add `verbose` logging mode for easier debugging.

PiperOrigin-RevId: 169934019
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Clean up and improve example text for snt.custom_getters.Context.]


```

---

## 2017-09-25T13:53:45Z -- Remove deprecated functions in shakespeare example. (`7da90b8e`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove deprecated functions in shakespeare example.
PiperOrigin-RevId: 169901199
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove deprecated functions in shakespeare example.]


```

---

## 2017-09-25T13:17:14Z -- Update changelog for 1.13 (`57e91bca`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog for 1.13
PiperOrigin-RevId: 169898474
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog for 1.13]


```

---

## 2017-09-25T13:16:49Z -- Sonnet version update produced on Monday, 25. September 2017 (`236de5af`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 25. September 2017
PiperOrigin-RevId: 169898437
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 25. September 2017]


```

---

## 2017-09-25T13:13:31Z -- Clarify readme example. (`21f552d4`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Clarify readme example.
PiperOrigin-RevId: 169898200
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Clarify readme example.]


```

---

## 2017-09-18T17:20:16Z -- Finalise the deprecation of batch norm support in snt.LSTM. (`a27371fc`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Finalise the deprecation of batch norm support in snt.LSTM.
snt.LSTM and snt.BatchNormLSTM are now separate implementations.

PiperOrigin-RevId: 169107225
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Finalise the deprecation of batch norm support in snt.LSTM.]


```

---

## 2017-09-18T13:37:20Z -- Internal change (`2a00d7c6`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 169081550
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2017-09-18T13:18:06Z -- Update changelog for 1.12 (`0d57eadb`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog for 1.12
PiperOrigin-RevId: 169080040
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog for 1.12]


```

---

## 2017-09-18T13:11:42Z -- Sonnet version update produced on Monday, 18. September 2017 (`7957296c`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 18. September 2017
PiperOrigin-RevId: 169079587
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 18. September 2017]


```

---

## 2017-09-15T17:21:38Z -- Provide warning in Sequential.get_variables() (`4f0321bd`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Provide warning in Sequential.get_variables()
PiperOrigin-RevId: 168851699
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Provide warning in Sequential.get_variables]


```

---

## 2017-09-15T13:17:27Z -- Create snt.custom_getters sub-package. (`8c61488e`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Create snt.custom_getters sub-package.
PiperOrigin-RevId: 168826905
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Create snt.custom_getters sub-package.]


```

---

## 2017-09-08T16:21:12Z -- Added dilated convolution option to ConvLSTM. (`6aca22ec`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added dilated convolution option to ConvLSTM.
Dilated convolutions are useful when you want a larger receptive field in fewer
number of layers. Since usually only a few ConvLSTM layers are ever stacked
together, adding the option for dilated convolutions in the LSTM seems
especially appropriate.

Due to the underlying conv operation already accepting a dilation rate keyword
argument, only minor changes are required for this functionality.

Added test for instantiating, connecting, and running a ConvLSTM with the
dilated convolution option.

PiperOrigin-RevId: 168005030
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added dilated convolution option to ConvLSTM.]


```

---

## 2017-09-08T11:11:14Z -- conv module: use `is_compatible_with` for type checks to support tf.float32_ref (`eb8feb0f`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
conv module: use `is_compatible_with` for type checks to support tf.float32_ref
type inputs

PiperOrigin-RevId: 167981266
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[conv module: use is_compatible_with for type checks to support tf.float32_ref]


```

---

## 2017-09-07T15:31:02Z -- Allow logging of variables with non-static shape. (`86bcebb0`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow logging of variables with non-static shape.
PiperOrigin-RevId: 167863175
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow logging of variables with non-static shape.]


```

---

## 2017-09-07T14:22:12Z -- Put example code in code block. (`ff9b8b4f`)

**Author:** sracaniere <sracaniere@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Put example code in code block.
PiperOrigin-RevId: 167856667
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Put example code in code block.]


```

---

## 2017-09-05T16:54:59Z -- Expose the trainable_initial_state() function as an export of the sonnet module. (`9aa801a7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose the trainable_initial_state() function as an export of the sonnet module.
PiperOrigin-RevId: 167591969
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose the trainable_initial_state function as an export of the sonnet module.]


```

---

## 2017-09-04T16:52:36Z -- Remove gpu installation form installation instructions. (`18ae92c0`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove gpu installation form installation instructions.
The gpu and non-gpu wheels are now the same. We will keep uploading gpu wheels for compatibility, but this option don't need to be stated in the readme.

PiperOrigin-RevId: 167505530
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove gpu installation form installation instructions.]


```

---

## 2017-08-31T12:55:37Z -- Use graph.collections instead of graph._collections in util.py. (`da08b05e`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use graph.collections instead of graph._collections in util.py.
PiperOrigin-RevId: 167127911
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use graph.collections instead of graph._collections in util.py.]


```

---

## 2017-08-31T11:08:49Z -- Remove Tensorflow submodule. (`44150045`)

**Author:** Diego de Las Casas <diegodelascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove Tensorflow submodule.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove Tensorflow submodule.]


```

---

## 2017-08-30T16:33:20Z -- Add submodule changes to changelog. (`8075f64d`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add submodule changes to changelog.
PiperOrigin-RevId: 167004985
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add submodule changes to changelog.]


```

---

## 2017-08-30T12:16:33Z -- Change setup.py.tmpl to generate a pure python distribution. (`146d12f6`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change setup.py.tmpl to generate a pure python distribution.
PiperOrigin-RevId: 166980596
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change setup.py.tmpl to generate a pure python distribution.]


```

---

## 2017-08-29T16:12:16Z -- Allow snt.Module to take custom_getter kwarg. (`cf66bdd2`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow snt.Module to take custom_getter kwarg.
PiperOrigin-RevId: 166855493
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow snt.Module to take custom_getter kwarg.]


```

---

## 2017-08-23T15:55:46Z -- Prepare code for removal of the tensorflow submodule. (`d8320ca4`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Prepare code for removal of the tensorflow submodule.
PiperOrigin-RevId: 166207029
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Prepare code for removal of the tensorflow submodule.]


```

---

## 2017-08-23T11:09:28Z -- Remove group_sliced_variables argument from snt.get_saver, and use var.op.name rather than var.name for checkpointing. (`8702b4cb`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove group_sliced_variables argument from snt.get_saver, and use var.op.name rather than var.name for checkpointing.
PiperOrigin-RevId: 166183847
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove group_sliced_variables argument from snt.get_saver and use var.op.name rather than var.name for checkpointing.]


```

---

## 2017-08-22T15:28:17Z -- Fix typo in setup.py.tmpl (`72a5676e`)

**Author:** Alan Descoins <alandescoins@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix typo in setup.py.tmpl
PiperOrigin-RevId: 166063376
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix typo in setup.py.tmpl]


```

---

## 2017-08-18T13:17:42Z -- Replace tf.contrib.rnn.RNNCell by snt.RNNCore. This removes tf.contrib import dependence from Sonnet. (`933e2190`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Replace tf.contrib.rnn.RNNCell by snt.RNNCore. This removes tf.contrib import dependence from Sonnet.
PiperOrigin-RevId: 165698601
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Replace tf.contrib.rnn.RNNCell by snt.RNNCore. This removes tf.contrib import dependence from Sonnet.]


```

---

## 2017-08-18T09:28:35Z -- Remove resampler from sonnet since it is now in tensorflow v1.3. (`cc3fc9be`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove resampler from sonnet since it is now in tensorflow v1.3.
PiperOrigin-RevId: 165685049
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove resampler from sonnet since it is now in tensorflow v1.3.]


```

---

## 2017-08-17T15:22:38Z -- Add custom_getter, missing initializer fields to AlexNet (`e720b306`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add custom_getter, missing initializer fields to AlexNet
PiperOrigin-RevId: 165580788
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add custom_getter missing initializer fields to AlexNet]


```

---

## 2017-08-16T18:26:44Z -- Move compatibility information from INSTALL to external README. (`e3e04ba0`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move compatibility information from INSTALL to external README.
PiperOrigin-RevId: 165469503
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move compatibility information from INSTALL to external README.]


```

---

## 2017-08-15T15:04:39Z -- Allows for specifying custom_getters in Sonnet RNN modules. (`1f49c217`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allows for specifying custom_getters in Sonnet RNN modules.
PiperOrigin-RevId: 165308303
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allows for specifying custom_getters in Sonnet RNN modules.]


```

---

## 2017-08-14T16:18:43Z -- Sonnet version update produced on Monday, 14. August 2017 (`e8cf2af1`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 14. August 2017
PiperOrigin-RevId: 165186514
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 14. August 2017]


```

---

## 2017-08-10T18:40:12Z -- Fixes bias compatibility between NHWC and NCHW data formats in Conv2D. (`b7ded90b`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixes bias compatibility between NHWC and NCHW data formats in Conv2D.
Uses tf.nn.bias_add for bias addition in all convolutional layers.

PiperOrigin-RevId: 164881421
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixes bias compatibility between NHWC and NCHW data formats in Conv2D.]


```

---

## 2017-08-08T19:04:00Z -- snt.BatchApply now also accepts scalar-valued inputs such as Boolean flags. (`9d84caae`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
snt.BatchApply now also accepts scalar-valued inputs such as Boolean flags.
PiperOrigin-RevId: 164625416
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[snt.BatchApply now also accepts scalar-valued inputs such as Boolean flags.]


```

---

## 2017-08-07T18:18:44Z -- First steps of AlexNet cleanup. (`4c62b40b`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
First steps of AlexNet cleanup.
- Add option to disable batch normalization on fully-connected layers.
- Remove HALF mode.
- Add AlexNetMini and AlexNetFull.

PiperOrigin-RevId: 164484078
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[First steps of AlexNet cleanup.]


```

---

## 2017-08-07T16:59:19Z -- Change readme instructions to use pip (`a03b1516`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change readme instructions to use pip
PiperOrigin-RevId: 164472799
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change readme instructions to use pip]


```

---

## 2017-08-07T16:58:05Z -- Adds **kwargs to util.get_saver for better options (`4d528c15`)

**Author:** Javier Rey <javierrey@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adds **kwargs to util.get_saver for better options
GIT_ORIGIN_REV_ID=064e7809822d97719e930be3d072a74c0f393491
PiperOrigin-RevId: 164472684
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adds kwargs to util.get_saver for better options]


```

---

## 2017-08-07T15:02:25Z -- Sonnet version update produced on Monday, 07. August 2017 (`d0e09838`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 07. August 2017
PiperOrigin-RevId: 164460642
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 07. August 2017]


```

---

## 2017-08-03T13:04:58Z -- Document dict ordering in nest and make it consistent with sonnet. (`4813caaa`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Document dict ordering in nest and make it consistent with sonnet.
PiperOrigin-RevId: 164114335
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Document dict ordering in nest and make it consistent with sonnet.]


```

---

## 2017-08-03T10:05:39Z -- Remove redundant tests and code (`ee5177b6`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove redundant tests and code
PiperOrigin-RevId: 164101282
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove redundant tests and code]


```

---

## 2017-08-01T15:24:05Z -- Add six to setup.py dependencies (`2af80eab`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add six to setup.py dependencies
PiperOrigin-RevId: 163831691
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add six to setup.py dependencies]


```

---

## 2017-07-31T18:06:51Z -- Remove config=opt from installation instructions. (`63d4b283`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove config=opt from installation instructions.
PiperOrigin-RevId: 163718326
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove configopt from installation instructions.]


```

---

## 2017-07-31T16:41:52Z -- Update changelog for 1.8 (`f0ac4cb4`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog for 1.8
PiperOrigin-RevId: 163704885
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog for 1.8]


```

---

## 2017-07-31T14:21:15Z -- Sonnet version update produced on Monday, 31. July 2017 (`05024f15`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 31. July 2017
PiperOrigin-RevId: 163690464
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 31. July 2017]


```

---

## 2017-07-26T16:39:19Z -- Add an optional multiplier for the bias in snt.AddBias. (`ca7be655`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add an optional multiplier for the bias in snt.AddBias.
This allows you to add and subtract the same bias in two different places, for example the encoder and decoder of an autoencoder.

PiperOrigin-RevId: 163217194
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add an optional multiplier for the bias in snt.AddBias.]


```

---

## 2017-07-24T17:09:48Z -- Sonnet version 1.7 update. (`65a8a047`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version 1.7 update.
PiperOrigin-RevId: 162951774
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version 1.7 update.]


```

---

## 2017-07-24T17:07:34Z -- Fix python3 installation - merge of GH PR #54 (`75b4969f`)

**Author:** guillaume-chevalier <guillaumechevalier@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix python3 installation - merge of GH PR #54
PiperOrigin-RevId: 162951451
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix python3 installation - merge of GH PR 54]


```

---

## 2017-07-20T16:49:21Z -- A more helpful error message from AbstractModule's _check_init_called (tells you the name of the offending class, which typically won't  appear in the stack trace). (`a4731184`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
A more helpful error message from AbstractModule's _check_init_called (tells you the name of the offending class, which typically won't  appear in the stack trace).
PiperOrigin-RevId: 162626708
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[A more helpful error message from AbstractModules _check_init_called tells you the name of the offending class which typically wont  appear in the stack trace.]


```

---

## 2017-07-18T13:29:53Z -- Use tf.layers.utils instead of tf.contrib.layers.utils in batch_norm. (`68de88eb`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use tf.layers.utils instead of tf.contrib.layers.utils in batch_norm.
PiperOrigin-RevId: 162344641
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use tf.layers.utils instead of tf.contrib.layers.utils in batch_norm.]


```

---

## 2017-07-17T17:12:07Z -- Sonnet version update produced on Monday, 17. July 2017 (`939d4f3c`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 17. July 2017
PiperOrigin-RevId: 162230152
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 17. July 2017]


```

---

## 2017-07-17T16:46:04Z -- Sonnet changelog update produced on Monday, 17. July 2017 (`6d6d18cd`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet changelog update produced on Monday, 17. July 2017
PiperOrigin-RevId: 162226163
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet changelog update produced on Monday 17. July 2017]


```

---

## 2017-07-17T10:31:50Z -- Remove unused py_func in masked convolution (`b4c3e267`)

**Author:** fbesse <fbesse@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove unused py_func in masked convolution
PiperOrigin-RevId: 162190992
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove unused py_func in masked convolution]


```

---

## 2017-07-14T11:00:25Z -- Rename wheels to "dm-sonnet" and "dm-sonnet-gpu". The import name "sonnet" remains unchanged. (`b70bf58f`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Rename wheels to "dm-sonnet" and "dm-sonnet-gpu". The import name "sonnet" remains unchanged.
PiperOrigin-RevId: 161937786
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Rename wheels to dm-sonnet and dm-sonnet-gpu. The import name sonnet remains unchanged.]


```

---

## 2017-07-12T17:39:28Z -- Better error messages for BatchReshape. (`f23c46d1`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Better error messages for BatchReshape.
PiperOrigin-RevId: 161684007
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Better error messages for BatchReshape.]


```

---

## 2017-07-12T16:49:24Z -- Support "None" entries in BatchApply's inputs. (`1d67fab8`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support "None" entries in BatchApply's inputs.
PiperOrigin-RevId: 161676977
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support None entries in BatchApplys inputs.]


```

---

## 2017-07-12T12:03:53Z -- Add `custom_getter` option to convolution modules. (`a73d3547`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add `custom_getter` option to convolution modules.
PiperOrigin-RevId: 161651983
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add custom_getter option to convolution modules.]


```

---

## 2017-07-11T19:46:50Z -- Allow MLP custom getter to be configured. (`d4c6ab42`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow MLP custom getter to be configured.
PiperOrigin-RevId: 161567563
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow MLP custom getter to be configured.]


```

---

## 2017-07-11T15:53:25Z -- Sonnet version update produced on Monday, 10. July 2017 (`28c77774`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 10. July 2017
PiperOrigin-RevId: 161534291
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 10. July 2017]


```

---

## 2017-07-07T12:24:01Z -- Add IPython notebook that explains how Sonnet's BatchNorm module can be (`5456e33a`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add IPython notebook that explains how Sonnet's BatchNorm module can be
configured.

PiperOrigin-RevId: 161191851
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add IPython notebook that explains how Sonnets BatchNorm module can be]


```

---

## 2017-07-07T09:49:39Z -- Accept string values as variable scope in snt.get_variables_in_scope and snt. get_normalized_variable_map. (`013b000d`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Accept string values as variable scope in snt.get_variables_in_scope and snt. get_normalized_variable_map.
PiperOrigin-RevId: 161183031
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Accept string values as variable scope in snt.get_variables_in_scope and snt. get_normalized_variable_map.]


```

---

## 2017-07-05T15:08:39Z -- fix package name in docstrings. (`5e582253`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
fix package name in docstrings.
PiperOrigin-RevId: 160958733
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[fix package name in docstrings.]


```

---

## 2017-07-04T11:19:47Z -- Fixes default name of CausalConv1D. (`2bd0193f`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixes default name of CausalConv1D.
PiperOrigin-RevId: 160882252
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixes default name of CausalConv1D.]


```

---

## 2017-07-03T13:49:00Z -- Added all constructor arguments to ConvNet2D.transpose and ConvNet2DTranspose.transpose. (`5bc33d9c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added all constructor arguments to ConvNet2D.transpose and ConvNet2DTranspose.transpose.
PiperOrigin-RevId: 160825947
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added all constructor arguments to ConvNet2D.transpose and ConvNet2DTranspose.transpose.]


```

---

## 2017-06-30T13:33:13Z -- is_training flags of _build functions no longer default to True (`8dfcb3b3`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
is_training flags of _build functions no longer default to True
PiperOrigin-RevId: 160639917
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[is_training flags of _build functions no longer default to True]


```

---

## 2017-06-30T10:28:20Z -- Add a causal 1D convolutional layer. (`6cbdb938`)

**Author:** tmramalho <tmramalho@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add a causal 1D convolutional layer.
PiperOrigin-RevId: 160630591
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add a causal 1D convolutional layer.]


```

---

## 2017-06-30T09:40:21Z -- Internal change (`aa81cfb4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 160627387
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2017-06-29T19:18:46Z -- Update how empty prefix string is handled to avoid accidentally removing the first letter of the scope name. (`cc8dd841`)

**Author:** fviola <fviola@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update how empty prefix string is handled to avoid accidentally removing the first letter of the scope name.
PiperOrigin-RevId: 160556971
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update how empty prefix string is handled to avoid accidentally removing the first letter of the scope name.]


```

---

## 2017-06-29T18:55:44Z -- Adding flatten_dict_items to snt.nest. (`b477b25c`)

**Author:** liusiqi <liusiqi@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adding flatten_dict_items to snt.nest.
PiperOrigin-RevId: 160554295
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adding flatten_dict_items to snt.nest.]


```

---

## 2017-06-29T04:18:37Z -- Conv1DTranspose modules can accept input with undefined batch sizes. (`ef86d2e7`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Conv1DTranspose modules can accept input with undefined batch sizes.
PiperOrigin-RevId: 160485877
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Conv1DTranspose modules can accept input with undefined batch sizes.]


```

---

## 2017-06-28T16:17:38Z -- Apply verification to output_shape in ConvTranspose modules; now allows the output_shape to be an integer. (`71048f7f`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Apply verification to output_shape in ConvTranspose modules; now allows the output_shape to be an integer.
PiperOrigin-RevId: 160415901
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Apply verification to output_shape in ConvTranspose modules now allows the output_shape to be an integer.]


```

---

## 2017-06-27T13:52:04Z -- Remove stale dependency on nose-parameterized (`1e7306c8`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove stale dependency on nose-parameterized
PiperOrigin-RevId: 160269659
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove stale dependency on nose-parameterized]


```

---

## 2017-06-26T17:58:06Z -- Add some features missed from 1.3 changelog (`05b739a9`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add some features missed from 1.3 changelog
PiperOrigin-RevId: 160165859
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add some features missed from 1.3 changelog]


```

---

## 2017-06-26T17:08:58Z -- Convert method name to snake_case when defining name scope. (`7d92cac0`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Convert method name to snake_case when defining name scope.
PiperOrigin-RevId: 160158998
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Convert method name to snake_case when defining name scope.]


```

---

## 2017-06-26T17:04:15Z -- Update changelog and version to 1.3 (`1981d56d`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog and version to 1.3
PiperOrigin-RevId: 160158383
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog and version to 1.3]


```

---

## 2017-06-26T14:19:16Z -- Restructure documentation (`2060928b`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Restructure documentation
PiperOrigin-RevId: 160141074
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Restructure documentation]


```

---

## 2017-06-26T12:47:31Z -- Move resampler from sonnet to contrib. (`77b40e2d`)

**Author:** yazhe <yazhe@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move resampler from sonnet to contrib.
PiperOrigin-RevId: 160134565
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move resampler from sonnet to contrib.]


```

---

## 2017-06-23T14:42:00Z -- Move name functions to utils. (`60be2bb4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move name functions to utils.
PiperOrigin-RevId: 159947742
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move name functions to utils.]


```

---

## 2017-06-22T18:02:43Z -- Add `custom_getter` option to snt.AbstractModule and snt.Linear. (`1b3434a8`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add `custom_getter` option to snt.AbstractModule and snt.Linear.
PiperOrigin-RevId: 159848731
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add custom_getter option to snt.AbstractModule and snt.Linear.]


```

---

## 2017-06-22T12:53:30Z -- Move tests of reuse_variables to utils.test. (`63681a63`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move tests of reuse_variables to utils.test.
PiperOrigin-RevId: 159816988
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move tests of reuse_variables to utils.test.]


```

---

## 2017-06-22T10:43:57Z -- Update Tensorflow submodule to 1.2. (`ffbaf275`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update Tensorflow submodule to 1.2.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update Tensorflow submodule to 1.2.]


```

---

## 2017-06-21T19:11:58Z -- Determine default snt.Module name from the passed callable. (`458ff962`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Determine default snt.Module name from the passed callable.
PiperOrigin-RevId: 159725156
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Determine default snt.Module name from the passed callable.]


```

---

## 2017-06-21T13:55:05Z -- Make RNNCore stop inheriting from RNNCell. (`6a1244d2`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make RNNCore stop inheriting from RNNCell.
PiperOrigin-RevId: 159688014
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make RNNCore stop inheriting from RNNCell.]


```

---

## 2017-06-21T11:25:43Z -- Internal change (`32395f9b`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 159678581
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2017-06-21T11:17:26Z -- Added default output shapes for ConvTranspose modules. (`5cca2678`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added default output shapes for ConvTranspose modules.
PiperOrigin-RevId: 159678052
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added default output shapes for ConvTranspose modules.]


```

---

## 2017-06-21T09:32:15Z -- remove explicit dependency on tensorflow 1.0.1 (`abf71e18`)

**Author:** Dustin Tran <dustintran@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
remove explicit dependency on tensorflow 1.0.1
GitOrigin-RevId=970a97e3a20e0ba53386b4d85141ade45ff7a89b
PiperOrigin-RevId: 159671292
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[remove explicit dependency on tensorflow 1.0.1]


```

---

## 2017-06-19T17:13:44Z -- Fix CUDA dependencies of bazel GPU build. (`ffa0e421`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix CUDA dependencies of bazel GPU build.
PiperOrigin-RevId: 159441644
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix CUDA dependencies of bazel GPU build.]


```

---

## 2017-06-19T15:44:44Z -- Fix bad docstring in BatchFlatten. Add test to assert behavior is as described. (`058eb113`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix bad docstring in BatchFlatten. Add test to assert behavior is as described.
PiperOrigin-RevId: 159431117
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix bad docstring in BatchFlatten. Add test to assert behavior is as described.]


```

---

## 2017-06-16T17:09:09Z -- Promote the @reuse_vars decorator out of experimental by moving it into util.py (`0944fc6e`)

**Author:** gabrielbm <gabrielbm@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Promote the @reuse_vars decorator out of experimental by moving it into util.py
PiperOrigin-RevId: 159241452
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Promote the reuse_vars decorator out of experimental by moving it into util.py]


```

---

## 2017-06-16T13:22:08Z -- Cleanup on LSTM/RNN state names in Sonnet. (`c345d9ea`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Cleanup on LSTM/RNN state names in Sonnet.
Renaming states from `prev` and `new` to `prev` and `next` for consistency with the traditional naming/convention used in basic_rnn.py and the documentation.

PiperOrigin-RevId: 159220895
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Cleanup on LSTM/RNN state names in Sonnet.]


```

---

## 2017-06-16T11:35:27Z -- Add documentation based on Sphinx (`3ba6a2a0`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add documentation based on Sphinx
The documentation can be generated by running `make docs` in the repository.
Output files can be found in the `docs/_build` directory.
The documentation currently reflects the contents of `README.md`.
Docstrings that use markdown are correctly rendered in the API documentation.
Pandoc and the pypandoc package need to be installed for this to work.

PiperOrigin-RevId: 159215223
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add documentation based on Sphinx]


```

---

## 2017-06-15T17:10:36Z -- Add preserve_dims parameter for BatchReshape. Multiple unknown dimensions are allowed too. (`b8a9a312`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add preserve_dims parameter for BatchReshape. Multiple unknown dimensions are allowed too.
PiperOrigin-RevId: 159118158
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add preserve_dims parameter for BatchReshape. Multiple unknown dimensions are allowed too.]


```

---

## 2017-06-15T15:52:47Z -- Add hidden / cell clipping to LSTM. (`c99083dd`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add hidden / cell clipping to LSTM.
PiperOrigin-RevId: 159109033
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add hidden / cell clipping to LSTM.]


```

---

## 2017-06-15T10:30:48Z -- Add check to Alexnet module that dropout is not being used when is_training flag is set to False. (`fb1d5a25`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add check to Alexnet module that dropout is not being used when is_training flag is set to False.
PiperOrigin-RevId: 159086181
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add check to Alexnet module that dropout is not being used when is_training flag is set to False.]


```

---

## 2017-06-14T17:59:45Z -- Set group_sliced_variables to True by default. (`a83fec34`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Set group_sliced_variables to True by default.
PiperOrigin-RevId: 158996130
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Set group_sliced_variables to True by default.]


```

---

## 2017-06-14T17:57:25Z -- Update docstrings for the partitioners and regularizers arguments (`0bd96d55`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update docstrings for the partitioners and regularizers arguments
PiperOrigin-RevId: 158995836
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update docstrings for the partitioners and regularizers arguments]


```

---

## 2017-06-14T17:02:35Z -- Print a warning when DeepRNN heuristic for output_size may be incorrect (`d8e6e4c2`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Print a warning when DeepRNN heuristic for output_size may be incorrect
PiperOrigin-RevId: 158991366
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Print a warning when DeepRNN heuristic for output_size may be incorrect]


```

---

## 2017-06-14T16:14:36Z -- Passes through inferred data type to bias and weight initializers. (`80d0f4af`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Passes through inferred data type to bias and weight initializers.
This results in more meaningful error messages if using non-float data types with the default initializer.

We could in future offer different default initializers for non-float types.

PiperOrigin-RevId: 158985788
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Passes through inferred data type to bias and weight initializers.]


```

---

## 2017-06-14T14:08:18Z -- Add .output_size property to MLP. This allows it to be used in DeepRNN. (`4a6873c1`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add .output_size property to MLP. This allows it to be used in DeepRNN.
PiperOrigin-RevId: 158973972
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add .output_size property to MLP. This allows it to be used in DeepRNN.]


```

---

## 2017-06-14T13:30:51Z -- Internal change (`e3b5eba8`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 158971193
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---

## 2017-06-13T15:36:59Z -- Remove deprecated properties in AbstractModule. (`8b7add67`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove deprecated properties in AbstractModule.
These have been providing deprecation warnings for quite a while now. If this
change breaks your code, the fix is changing all .name references to .scope_name, and
all .var_scope references to .variable_scope

PiperOrigin-RevId: 158849184
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove deprecated properties in AbstractModule.]


```

---

## 2017-06-13T14:26:04Z -- Ensure sensible snake_case conversions of CamelCase. (`9f148d1c`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure sensible snake_case conversions of CamelCase.
PiperOrigin-RevId: 158842782
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure sensible snake_case conversions of CamelCase.]


```

---

## 2017-06-13T12:41:24Z -- Fix typos and formatting in changelog (`d886aaf1`)

**Author:** diegolascasas <diegolascasas@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix typos and formatting in changelog
PiperOrigin-RevId: 158835303
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix typos and formatting in changelog]


```

---

## 2017-06-12T18:04:47Z -- Add Changelog and bump version to 1.1 (`3fd7d9d3`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Changelog and bump version to 1.1
PiperOrigin-RevId: 158735890
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Changelog and bump version to 1.1]


```

---

## 2017-06-12T17:35:15Z -- Add statement in contrib directory. (`f3921b17`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add statement in contrib directory.
PiperOrigin-RevId: 158731193
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add statement in contrib directory.]


```

---

## 2017-06-12T16:47:33Z -- Cause Sonnet modules to throw an error if pickled. (`542e5da9`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Cause Sonnet modules to throw an error if pickled.
PiperOrigin-RevId: 158724425
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Cause Sonnet modules to throw an error if pickled.]


```

---

## 2017-06-12T15:08:48Z -- Clarify readme that serializing Sonnet modules is not supported. (`5d6357f6`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Clarify readme that serializing Sonnet modules is not supported.
PiperOrigin-RevId: 158713914
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Clarify readme that serializing Sonnet modules is not supported.]


```

---

## 2017-06-12T14:40:08Z -- Improve coverage stats. (`df53261f`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve coverage stats.
* Add missing test.
* Remove check for situation that can never occur. If
self._graph is not defined this means __init__ has
not yet been called. This is an error and will be
caught by self._check_init_called().
* Remove unnecessary use of pass.

PiperOrigin-RevId: 158711427
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve coverage stats.]


```

---

## 2017-06-12T13:55:16Z -- Use tolerance to fix initializers_test. (`4c6bdcfc`)

**Author:** mareynolds <mareynolds@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use tolerance to fix initializers_test.
PiperOrigin-RevId: 158708184
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use tolerance to fix initializers_test.]


```

---

## 2017-06-05T14:56:05Z -- Use class name as the default module name. (`4437dcfb`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use class name as the default module name.
* If no module name provided, use the class name
converted to snake_case (without leading underscores).
* Raise a TypeError on a type error instead of a
ValueError.

PiperOrigin-RevId: 158014533
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use class name as the default module name.]


```

---

## 2017-06-04T14:15:43Z -- Add Residual and SkipConnection wrappers. (`51506397`)

**Author:** arahuja <arahuja@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add Residual and SkipConnection wrappers.
PiperOrigin-RevId: 157958992
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add Residual and SkipConnection wrappers.]


```

---

## 2017-06-02T16:19:43Z -- Add a group_sliced_variables option to get_normalized_variable_map() that groups partitioned variables in its return value, in line with what tf.Saver expects to receive. This ensures that partitioned variables end up being treated as a unit when saving checkpoints / model snapshots with tf.Saver. The option is set to False by default, for backwards compatibility reasons. (`1bfe7c57`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add a group_sliced_variables option to get_normalized_variable_map() that groups partitioned variables in its return value, in line with what tf.Saver expects to receive. This ensures that partitioned variables end up being treated as a unit when saving checkpoints / model snapshots with tf.Saver. The option is set to False by default, for backwards compatibility reasons.
PiperOrigin-RevId: 157838363
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add a group_sliced_variables option to get_normalized_variable_map that groups partitioned variables in its return value in line with what tf.Saver expects to receive. This ensures that partitioned variables end up being treated as a unit when saving checkpoints / model snapshots with tf.Saver. The option is set to False by default for backwards compatibility reasons.]


```

---

## 2017-06-01T16:55:45Z -- Add support for named arguments and nested dictionaries to snt.BatchApply, and support None in return. (`8c6540d2`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add support for named arguments and nested dictionaries to snt.BatchApply, and support None in return.
PiperOrigin-RevId: 157726045
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add support for named arguments and nested dictionaries to snt.BatchApply and support None in return.]


```

---

## 2017-05-31T21:57:28Z -- Fix typos in mlp.py and convnet.py. (`03ada1f8`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix typos in mlp.py and convnet.py.
PiperOrigin-RevId: 157639586
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix typos in mlp.py and convnet.py.]


```

---

## 2017-05-30T17:34:31Z -- Use nest functions from tensorflow.contrib (`b3a4c2c9`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use nest functions from tensorflow.contrib
PiperOrigin-RevId: 157480641
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use nest functions from tensorflow.contrib]


```

---

## 2017-05-25T14:53:03Z -- snt.Linear.transpose now uses snt.Linear's partitioners (`1a7968a2`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
snt.Linear.transpose now uses snt.Linear's partitioners
PiperOrigin-RevId: 157107275
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[snt.Linear.transpose now uses snt.Linears partitioners]


```

---

## 2017-05-24T16:24:15Z -- Separate graph construction from training in RNN example. (`48448e1d`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Separate graph construction from training in RNN example.
PiperOrigin-RevId: 156997379
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Separate graph construction from training in RNN example.]


```

---

## 2017-05-23T15:12:52Z -- Adds a log_variables() function that logs all variables with their shapes, types, collections, and device assignments. (`e765cae4`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adds a log_variables() function that logs all variables with their shapes, types, collections, and device assignments.
PiperOrigin-RevId: 156863114
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adds a log_variables function that logs all variables with their shapes types collections and device assignments.]


```

---

## 2017-05-22T14:20:18Z -- Correct constructor docstring for AbstractModule. (`40995a58`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Correct constructor docstring for AbstractModule.
PiperOrigin-RevId: 156734036
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Correct constructor docstring for AbstractModule.]


```

---

## 2017-05-18T14:23:58Z -- Improve snt.BatchNorm documentation. (`ea9965ee`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Improve snt.BatchNorm documentation.
PiperOrigin-RevId: 156428584
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Improve snt.BatchNorm documentation.]


```

---

## 2017-05-16T13:50:48Z -- Add link to TensorFlow docs for VALID/SAME and fix existing links. (`2c70faf3`)

**Author:** tfgg <tfgg@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add link to TensorFlow docs for VALID/SAME and fix existing links.
PiperOrigin-RevId: 156176530
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add link to TensorFlow docs for VALID/SAME and fix existing links.]


```

---

## 2017-04-28T18:09:54Z -- fixes for python3 compatibility (`578e3360`)

**Author:** Bj?rn Linse <bjrnlinse@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
fixes for python3 compatibility
GitOrigin-RevId=083515c1b58437e98c4ebd5935bd791d31a3a007
PiperOrigin-RevId: 154559781
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[fixes for python3 compatibility]


```

---

## 2017-04-27T21:27:38Z -- Fix Clone tests to be compatible with TF 1.0.1. (`23972bbf`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix Clone tests to be compatible with TF 1.0.1.
PiperOrigin-RevId: 154470055
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix Clone tests to be compatible with TF 1.0.1.]


```

---

## 2017-04-27T16:44:10Z -- 1. Add a clone method to snt.Conv2D (`4afced7c`)

**Author:** sracaniere <sracaniere@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
1. Add a clone method to snt.Conv2D
2. Add property getters for mask and data_format

PiperOrigin-RevId: 154433148
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[1. Add a clone method to snt.Conv2D]


```

---

## 2017-04-26T13:31:14Z -- Implementation of Layer Normalization (https://arxiv.org/abs/1607.06450) as a Sonnet module. (`b31ec4d6`)

**Author:** liusiqi <liusiqi@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Implementation of Layer Normalization (https://arxiv.org/abs/1607.06450) as a Sonnet module.
PiperOrigin-RevId: 154290279
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Implementation of Layer Normalization https://arxiv.org/abs/1607.06450 as a Sonnet module.]


```

---

## 2017-04-25T16:40:37Z -- Allow passing in a string to tf.get_variables_in_scope. (`a0b0d630`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow passing in a string to tf.get_variables_in_scope.
PiperOrigin-RevId: 154182793
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow passing in a string to tf.get_variables_in_scope.]


```

---

## 2017-04-21T12:29:09Z -- Remove shortDescription tests. (`c9f0da7f`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove shortDescription tests.
PiperOrigin-RevId: 153817756
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove shortDescription tests.]


```

---

## 2017-04-21T10:36:29Z -- Change rnn_shakespeare_test to depend on rnn_shakespeare. (`edfce8ea`)

**Author:** adriap <adriap@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Change rnn_shakespeare_test to depend on rnn_shakespeare.
PiperOrigin-RevId: 153811930
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Change rnn_shakespeare_test to depend on rnn_shakespeare.]


```

---

## 2017-04-19T10:26:48Z -- Fix doc-level comment for batch_norm_test. (`5ab8510d`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix doc-level comment for batch_norm_test.
PiperOrigin-RevId: 153570174
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix doc-level comment for batch_norm_test.]


```

---

## 2017-04-06T12:05:06Z -- Merge internal changes. (`de33c8a5`)

**Author:** Deepmind <deepmind@users.noreply.github.com>

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge internal changes.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge internal changes.]


```

---

