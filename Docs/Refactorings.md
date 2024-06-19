# Refactorings

## Extract Function

This refactoring is similar to Extract Method [Fowlwer](), except that we extract the code to a free function instead of a method.

### Example

```c++
// before
namespace math {
  class vec {
  public:
    int x{0};
    int y{0};
    void normalize() {
      if (!has_length()) {
        throw std::runtime_error{"Cannot normalize a vector with 0 length."};
      }
      const auto length{std::sqrt(x * x + y * y)};
      x /= length;
      y /= length;
    }
  private:
    auto has_length() const {
      return x != 0 && y != 0;
    }
  };
}
```

```c++
// after
// header file
namespace math {
  class vec {
  public:
    int x{0};
    int y{0};
    void normalize();
  };
}

// source file
namespace math {
  namespace {
    auto has_length(const vec& v) const {
      return v.x != 0 && v.y != 0;
    }
  }
  void vec::normalize() {
    if (!has_length(*this)) {
      throw std::runtime_error{"Cannot normalize a vector with 0 length."};
    }
    const auto length{std::sqrt(x * x + y * y)};
    x /= length;
    y /= length;
  }
}
```
