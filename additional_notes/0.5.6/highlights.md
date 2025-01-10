
### Updated viewer handedness ✋

So. Funny story. 😅

For (checks notes) 5 years or so, napari has had a 3D view, and for those 5
years, for almost all datasets, that view has been a *mirror image* of the 3D
object they were trying to represent. Any biologists among you might have
noticed that loading 3D molecular coordinates of DNA would result in a
left-handed helix, while anatomists among you might have been surprised by how
many of your samples suffered from [situs inversus
totalis](https://en.wikipedia.org/wiki/Situs_inversus)! 

By and large, many things that people care about work exactly the same in the
mirror world — volume measurements, forces, tracking, speed, ... — so this bug
has gone mostly unnoticed, or noticed and shrugged off and unfixed for all this
time. But it's important for some things!  Your heart is on the left side of
your body, but the right side of your mirror image's. This can be critical, for
example, when using software to plan surgery! Thankfully, we are not aware of
any cases of napari being used in this way. 😅

napari uses zyx coordinates instead of xyz because it is the most natural way
to work with NumPy arrays and the rest of the scientific Python imaging
ecosystem. Flipping the axes in this way also changes the *handedness* of the
space, *unless* you also flip the direction of one of the dimensions. The
simplest way to illustrate this is [this 3D model of a right
shoe](https://grabcad.com/library/anatomic-shoe-sole-euro-right-41-1), which looks
like this in previous versions of napari:

![right shoe rendered as a left shoe in napari](https://github.com/user-attachments/assets/c9190e2c-f35a-44d1-95d5-f9877dd4c843)

and in 0.5.6+, thanks to [#7488](https://github.com/napari/napari/pull/7488):

![right shoe correctly rendered as a right shoe in napari](https://github.com/user-attachments/assets/e187f5e7-8e4a-4526-bae9-80a9bec6fea3)

Most users won't notice. But if you were among the users that noticed and you
implemented workarounds in your code (such as setting the z-scale to a negative
number), now is a good time to undo the workarounds for newer versions of
napari! If you run into any issues please get in touch [on GitHub
issues](https://github.com/napari/napari) or on our [Zulip chat room](https://napari.zulipchat.com)!

### Faster shapes 🚀

For its whole history, napari has been a pure Python package. As we go deeper
into its performance bottlenecks, though, we're finding that we need some
compiled code. This is a big change to the napari installation story, though,
so we are rolling it out slowly. But if you've been waiting forever to load
your shapes data, this release has some enhancements for you (>2x speedup)!
([#7346](https://github.com/napari/napari/pull/7346))

To use this speedup, you'll need to:
- install napari core developer Grzegorz Bokota's collection of performant
  algorithms,
  [PartSegCore-compiled-backend](https://pypi.org/project/PartSegCore-compiled-backend/).
  (you can install it automatically by pip installing `"napari[optional]"`.)
- *and*, in the napari advanced settings, tick the "Use C++ code to speed up
  creation and updates of Shapes layers" box.

Please give it a try and let us know if you encounter any issues! This is the
beginning of a new era of performance improvements in napari, to help it live
up to its promise of a *fast* viewer for n-dimensional data in Python!

### Other improvements

Often, the important information in a layer name is at the *end* of the name
rather than the beginning. We've improved the eliding (…) of long names by
placing the ellipsis in the *middle* of the name rather than the end
([#7461](https://github.com/napari/napari/pull/7461)).

The default value of "flash" has been changed to `False` in
`viewer.screenshot`, so that taking many screenshots in a script will not
result in rapid flickering
([#7476](https://github.com/napari/napari/pull/7476)). This is part of a
broader accessibility initiative by recent contributor [Tim
Monko](https://github.com/TimMonko) to improve napari for light-sensitive
users ([#7433](https://github.com/napari/napari/issues/7433), and we are so
grateful! 🙏
