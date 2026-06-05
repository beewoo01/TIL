# Basic

basic_provider
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

final helloProvider = Provider<String>((ref) {
  ref.onDispose(() {
    print('[helloProvider] disposed');
  });
  return 'Hello';
});

final worldProvider = Provider<String>((ref) {
  ref.onDispose(() {
    print('[worldProvider] disposed');
  });
  return 'World';
});

```

basic_page
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:provider_practice/pages/basic/basic_provider.dart';

class BasicPage extends ConsumerWidget {
  const BasicPage({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final hello = ref.watch(helloProvider);
    final world = ref.watch(worldProvider);
    return Scaffold(
      appBar: AppBar(title: const Text('Provider')),
      body: Center(
        child: Text('$hello $world kkkkkk', style: Theme.of(context).textTheme.headlineLarge,)
      ),
    );
  }
}

```

기본은 ConsumerWidget을 구현하던지, 따로 정의해서 Child로 사용하던지가 있다.
ref.watch를 통해 구독할 provider를 등록하고 사용하면 된다.
주의점은 자동으로 dispose가 안된다.
그렇게 되면 메모리 관리에서 어려움을 느낄것이다.
화면을 나갈 때 자동으로 메모리를 정리하고 싶다면 .autoDispose를 사용하면 된다.


