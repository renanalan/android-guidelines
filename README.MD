# Projeto Android

Bibliotecas e ferramentas:

- Support libraries
- RecyclerViews and CardViews 
- [RxJava](https://github.com/ReactiveX/RxJava) and [RxAndroid](https://github.com/ReactiveX/RxAndroid) 
- [Retrofit 2](http://square.github.io/retrofit/)
- [Dagger 2](http://google.github.io/dagger/)
- [Butterknife](https://github.com/JakeWharton/butterknife)
- [Timber](https://github.com/JakeWharton/timber)
- [Glide](https://github.com/bumptech/glide)
- Functional tests with [Espresso](https://google.github.io/android-testing-support-library/docs/espresso/index.html)
- [Robolectric](http://robolectric.org/)
- [Mockito](http://mockito.org/)
- [Checkstyle](http://checkstyle.sourceforge.net/), [PMD](https://pmd.github.io/) and [Findbugs](http://findbugs.sourceforge.net/) for code analysis

## Requirements

- JDK 1.8
- [Android SDK](http://developer.android.com/sdk/index.html).
- Android N [(API 24) ](http://developer.android.com/tools/revisions/platforms.html).
- Última versão do Android SDK Tools and build tools.


## Arquitetura

Este projeto segue as diretrizes de arquitetura do Android da Iteris baseadas em [MVP (Model View Presenter)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter).

https://github.com/renanalan/android-guidelines/blob/master/architecture_diagram_edit.png

### Como implementar uma nova tela MVP
Imagine que você precisa implementar uma tela de login.

1. Crie um novo pacote sob `ui` chamado` signin`.
2. Crie uma nova activity chamada `ActivitySignIn`. Você também pode usar um Fragment.
3. Defina a interface de visualização que sua atividade vai implementar. Crie uma nova interface chamada `SignInMvpView` que se herde de ` MvpView`. Adicione os métodos que você acha necessários. Exemplo: `showSignInSuccessful ()`
4. Crie uma classe `SignInPresenter` que herde `BasePresenter <SignInMvpView>`
5. Implemente os métodos no `SignInPresenter` que sua activity requer para executar as ações necessárias, e. `signIn (String email)`. Uma vez que sua requisição retorne sucesso, você deve chamar `getMvpView().showSignInSuccessful()`.
6. Crie um `SignInPresenterTest` e escreva testes de unitários para` signIn(email)`. Lembre-se de efetuar o mock em `SignInMvpView` e também o` DataManager`.
7. Faça a `ActivitySignIn` implementar `SignInMvpView` e implemente os métodos necessários como `showSignInSuccessful()`
8. Na sua activity, injete uma nova instância do `SignInPresenter` e chame o método `presenter.attachView (this) `de` onCreate` e `presenter.detachView ()` de `onDestroy ()`. Além disso, configure um listener de clique em seu botão que chama `presenter.signIn (email)`.

