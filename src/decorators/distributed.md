# @distributed

The `distributed` decorator enables function execution on a remote cluster. Synchronization works similarly to the [@concurrent](/decorators/concurrent.md) decorator.

```gyro

@distributed fn fibonacci(n: int): int {
  // Make sure our roles are clear
  if self.slave() {
    
  }

  if n <= 0 {
    return 0
  } elif n == 1 {
    return 1
  }
  
  return fibonacci(n - 1) + fibonacci(n - 2)
}

var result = fibonacci<targets = [127.0.0.1:4000, 127.0.0.1:4001, 127.0.0.1:4002]>()

```