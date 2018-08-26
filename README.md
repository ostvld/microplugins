# Microplugins
A C++17-based plugin framework for easy creating and using plugins.
It is supports services for plugins and communications between
plugins kernel and regular plugins.

Simple example:
```c++
#include <microplugins/plugins.hpp>


static std::any service(std::any a1) {
  std::shared_ptr<micro::plugins> k = std::any_cast<std::shared_ptr<micro::plugins>>(a1);

  std::shared_ptr<micro::iplugin> plugin1 = k->get_plugin("plugin1");

  if (plugin1 && plugin1->has<2>("sum2")) {
    std::shared_future<std::any> result = plugin1->run<2>(125, 175);
    result.wait();
    std::cout << std::any_cast<int>(result.get()) << std::endl;
  }

  k->stop(); // exit if we don't need service mode
  return 0;
}


int main() {
  std::shared_ptr<micro::plugins> k = micro::plugins::get();
  k->subscribe<1>("service", service);

  k->run();

  while (k->is_run()) micro::sleep<micro::milliseconds>(250);

  return k->error();
}
```

You can see [example of plugin](examples/plugin1.cxx), and [example of service](examples/microservice.cxx)

# Requirements
* GCC with support C++17 standart (including experimental/filesystem)
- Cmake >= 2.6 (for build examples)
- Doxygen (for build documentation)

# License
This library is distributed under the terms of the [Boost Software License](http://www.boost.org/LICENSE_1_0.txt)