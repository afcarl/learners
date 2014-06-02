learners
========

A small library for incremental learning of forward and inverse models, compatible with the [`environments`](https://github.com/humm/environments) module.


### OpenScience License

This software is placed under the [OpenScience license](http://fabien.benureau.com/openscience.html), which is the LGPL, with the additional condition that if you publish scientific results using this code, you have to publish the corresponding modifications of the code.

> If you publicly release any scientific claims or data that were supported or generated by the Program or a modification thereof, in whole or in part, you will release any modifications you made to the Program. This License will be in effect for the modified program.



# Learners: Tutorial

The `learners` module is organized arround the notion of channels. A `Channel`
has a name, describes a single scalar and can incorportate bounds. Here we
describe three channels, *x*, *y* and *a*, with bounds `[0, 10]` for the first
two and `[0, 100]` for *a*.

```python
    import forest
    from learners import Channel, RandomLearner

    ch_x = Channel('x', [0, 10])
    ch_y = Channel('y', [0, 10])
    ch_a = Channel('a', [0, 100])
```

Then we create a learner that accept *x* and *y* as motor input, and learns
their mapping to sensory channel *a*. We create the configuration and
instanciate a `RandomLearner` instance, that return a random prediction.


```python
    cfg = {'m_channels': [ch_x, ch_y],
           's_channels': [ch_a]}

    learner = RandomLearner(cfg)
```

We can then update the learner with an observation, a pair of motor and sensory
signal.

```python
    learner.update({'x': 5, 'y': 4}, {'a': 9})
```

We can then ask the learner to predict the result of a motor signal.

```python
    learner.predict({'x': 3, 'y': 4})
```
Output:
```python
    {'a': 14.046620079802707}
```


Or infer the motor command that should produce a certain sensory output.

```python
    learner.infer({'a': 3})
```
Output:
```python
    {'x': 3.9660529699286418, 'y': 3.132155464083369}
```

Here the learner is returning random signal, and thus is particularly useless.
