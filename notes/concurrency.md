# concurrency notes

A series of key tools in belt here for handling concurrent tasks, shared access to memory, and thread safety.

- Mutex can be used to coodinate shared data strctures across multiple threads. Unable to access the data except when you (in control flow) hold the lock. Locks release automatically.
- Read-only data can be shared among several threads and non-modification is enforced at compile time, preventing errors here.
- After transferring ownership of some data structure to another thread (move), the compiler ensure that you have relinquished all access to this by enforcing non-modification of it thereafter.


## useful stuff
- crossbeam crate, lets you spawn tasks in parallel. e.g.
```rust
  let threads = 8;
  let rows_per_band = bounds.1 / threads + 1;
  {
      let bands: Vec<&mut [u8]> = pixels.chunks_mut(rows_per_band * bounds.0).collect();
      crossbeam::scope(|spawner| {
          for (i, band) in bands.into_iter().enumerate() {
              let top = rows_per_band * i;
              let height = band.len() / bounds.0;
              let band_bounds = (bounds.0, height);
              let band_upper_left = pixel_to_point(bounds, (0, top), upper_left, lower_right);
              let band_lower_right =
                  pixel_to_point(bounds, (bounds.0, top + height), upper_left, lower_right);

              spawner.spawn(move |_| {
                  render(band, band_bounds, band_upper_left, band_lower_right);
              });
          }
      })
      .unwrap();
  }
```
Above example uses `crossbeam::scope` which takes a closure spawner, which can be used to capture variable scope in our example loop generates rows of an image to operate on.
The `spawner.spawn` function once again uses a closure to move values into its scope (safe concurrent memory practise) and performs some task.
`crossbeam::scope` ensures that all spawned tasks are completed before the scope is exited.
