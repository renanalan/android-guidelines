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
- [Espresso](https://google.github.io/android-testing-support-library/docs/espresso/index.html)
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

![](https://github.com/renanalan/android-guidelines/blob/master/architecture_diagram_edit.png)

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

# 1. Guidelines do projeto

## 1.1 Nome de arquivos

### 1.2.1 Classes
Classes são escritas em [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

Para classes que herdam componentes Android, o nome da classe deve terminar com o nome do componente. Exemplo: `SignInActivity`, `SignInFragment`.

### 1.2.2 Resources

Resources são escritos em __lowercase_underscore__. Exemplo: `activity_signin`, `fragment_signin`

#### 1.2.2.1 Arquivos Drawable

Convenções de nome para drawable:


| Asset        | Prefix            |		Exemplo          |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	           | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	           | `ic_star.png`               |
| Menu         | `menu_	`          | `menu_submenu_bg.9.png`     |
| Notification | `notification_`   | `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

Convenções de nome para ícones (baseado [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):

| Asset                           | Prefixo            | Exemplo                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

Convenções de nome para seletor de estados:

| Estado       | Sufixo          | Exemplo                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.2.2.2 Arquivos de layout

Os arquivos de layout devem corresponder ao nome dos componentes do Android para os quais eles se destinam, mas devemos colocar o nome do componente de para o início.

| Componente       | Nome da Classe         | Nome do Layout                |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

Um caso ligeiramente diferente é quando criamos um layout que será inflado por um `Adapter`, por exemplo, para preencher um` ListView`. Nesse caso, o nome do layout deve começar com `item_`.

#### 1.2.2.3 Arquivos de menu

Semelhante aos arquivos de layout, os arquivos de menu devem corresponder ao nome do componente. Por exemplo, se estivermos definindo um arquivo de menu que será usado no `UserActivity`, o nome do arquivo deve ser` activity_user.xml`

Uma boa prática é não incluir a palavra `menu` como parte do nome porque esses arquivos já estão localizados no diretório` menu`.

#### 1.2.2.4 Arquivos de valores (values)

Os arquivos de recursos na pasta de valores devem ser __plural__, e. `strings.xml`,` styles.xml`, `colors.xml`,` dimens.xml`, `attrs.xml`

# 2 Guidelines de codificação

## 2.1 Regra da linguagem Java

### 2.1.1 Não ignore as excessões

Você nunca deve fazer o seguinte:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_Como você pode pensar que seu código nunca encontrará essa condição de erro ou que não é importante lidar com isso, ignorar exceções como acima cria minas em seu código para que outra pessoa viaje durante algum dia. Você deve lidar com todas as Exceções em seu código de alguma maneira baseada em princípios. A manipulação específica varia dependendo do case._ - ([Diretrizes de estilo de código Android) (https://source.android.com/source/code-style.html))

### 2.1.2 Nunca pegue exceção genérica

Você não deve fazer isso:

```java
try {
    someComplicatedIOFunction();        // throw IOException
    someComplicatedParsingFunction();   // throw ParsingException
    someComplicatedSecurityFunction();  // throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // Pegará todas as excessões
    handleError();                      // com apenas um tratamento.
}
```

### 2.1.3 Importação

Errada: `import foo.*;`

Correta: `import foo.Bar;`

Veja mais [aqui](https://source.android.com/source/code-style.html#fully-qualify-imports)

## 2.2 Regra de estilo code Java

### 2.2.1 Trate as siglas como palavras

| Correto        | Errado         |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.2 Logs

Use os métodos de registro fornecidos pela classe `Log` para imprimir mensagens de erro ou outras informações que podem ser úteis para desenvolvedores identificar problemas:

* `Log.v(String tag, String msg)` (verbose)
* `Log.d(String tag, String msg)` (debug)
* `Log.i(String tag, String msg)` (information)
* `Log.w(String tag, String msg)` (warning)
* `Log.e(String tag, String msg)` (error)

Como regra geral, usamos o nome da classe como tag e definimos como um campo `static final` na parte superior do arquivo. Por exemplo:

```java
public class MyClass {
    private static final String TAG = MyClass.class.getSimpleName();

    public myMethod() {
        Log.e(TAG, "My error message");
    }
}
```

Os registros VERBOSE e DEBUG devem ser desabilitados nas versões de lançamento. Também é recomendado desativar os registros de INFORMAÇÃO, ADVERTÊNCIA e ERRO, mas você pode querer mantê-los habilitados se você achar que eles podem ser úteis para identificar problemas em versões de lançamento. Se você decidir deixá-los habilitados, você deve garantir que eles não estejam vazando informações privadas, como endereços de e-mail, IDs de usuários, etc.

Para mostrar apenas logs em compilações de depuração:

```java
if (BuildConfig.DEBUG) Log.d(TAG, "The value of x is " + x);
```

### 2.2.10 Ordenação dos membros de uma classe

Não há uma única solução correta para isso, é usando uma ordem __logical__ e __consistent__ melhorará a capacidade de leitura e legibilidade do código. É recomendável usar a seguinte ordem:

1. Constantes
2. Fields
3. Construtores
4. Override métodos e callbacks
5. Métodos públicos
6. Métodos privados
7. Classes aninhadas ou interfaces

Exemplo:

```java
public class MainActivity extends Activity {

    private static final String TAG = MainActivity.class.getSimpleName();

    private String mTitle;
    private TextView mTextViewTitle;

    @Override
    public void onCreate() {
        ...
    }

    public void setTitle(String title) {
    	mTitle = title;
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {

    }

}
```

Se a sua classe está estendendo um componente __Android, como uma Activity ou um Fragmento, é uma boa prática ordenar os métodos de substituição para que eles __match o ciclo de vida do componente__. Por exemplo, se você possui uma atividade que implementa`onCreate()`, `onDestroy()`, `onPause()` e `onResume()`, então o correto seria:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.11 Ordenação do parâmetro em um método

Ao programar para o Android, é bastante comum definir métodos que tenham um Contexto. Se você estiver escrevendo um método como este, o __Context__ deve ser o __primeiro__ parâmetro .

O caso oposto são __callback__ interfaces que sempre devem ser o __último__ parâmetro.

Exemplos:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.13 Contantes de String, nomes, e valores

Muitos elementos do SDK do Android, como `SharedPreferences`,` Bundle` ou `Intent`, usam uma abordagem de par chave-valor, portanto, é muito provável que, mesmo para um pequeno aplicativo, você acabe tendo que escrever muitas constantes de String.

Ao usar um desses componentes, você deve definir as chaves como campos `static final` e devem ser prefixados como indicado abaixo.

| Elemento           | Prefixo             |
| -----------------  | -----------------   |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

Observe que os argumentos de um Fragment - `Fragment.getArguments ()` - também são um Bundle. No entanto, como isso é um uso bastante comum de Bundles, nós definimos um prefixo diferente para eles.

Exemplo:

```java
// Observe que o valor do campo é o mesmo que o nome para evitar problemas de duplicação
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Itens relacionados à intenção usam o nome completo do pacote como valor
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.14 Arguments in Fragments and Activities

Quando os dados são passados para uma `Activity` ou `Fragment` por meio de uma `Intent` ou de um `Bundle`, as chaves dos diferentes valores __devem__ seguir as regras descritas na seção acima.

Quando uma `Activity` ou `Fragment` espera argumentos, ele deve fornecer um método `public static` que facilite a criação do relevante `Intent` ou `Fragment`.

No caso de Atividades, o método geralmente é chamado `startIntent()`:

```java
public static Intent startIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

Para Fragmentos, ele é chamado `newInstance ()` e lida com a criação do Fragment com os argumentos certos:

```java
public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment();
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args)
	return fragment;
}
```

__Note 1__: Esses métodos devem ir no topo da classe antes de `onCreate ()`.

__Note 2__: Se fornecemos os métodos descritos acima, as chaves para extras e argumentos devem ser `private` porque não é necessário que elas sejam expostas fora da classe.

## 2.3 XML

### 2.3.1 Usar self closing tags

Quando um elemento XML não tem nenhum conteúdo, você deve usar tags de auto-fechamento.

Correto:

```xml
<TextView
	android:id="@+id/tv_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

Errado :

```xml
<!-- Não faça isso! -->
<TextView
    android:id="@+id/tv_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```


### 2.3.2 Nome de resources

Resource IDs devem serem escritas em __lowercase_underscore__.

#### 2.3.2.1 Nomeando IDs

IDs devem seguir o seguinte prefixo:


| Elemento           | Prefixo           |
| -----------------  | ----------------- |
| `TextView`           | `tv_`             |
| `ImageView`          | `iv_`            |
| `Button`             | `bt_`           |
| `Menu`               | `mn_`             |

Imagem:

```xml
<ImageView
    android:id="@+id/iv_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu:

```xml
<menu>
	<item
        android:id="@+id/mn_done"
        android:title="Done" />
</menu>
```

#### 2.3.2.2 Strings

Strings começam com um prefixo que identifica a seção a que pertencem. Por exemplo `registration_email_hint` ou` registration_name_hint`. Se uma string __não pertencesse a nenhuma seção, então você deve seguir as regras abaixo__:


| Prefixo            | Descrição                             |
| -----------------  | --------------------------------------|
| `error_`             | Mensagem de erro                    |
| `msg_`               | Mensagem regular                    |
| `title_`             | Um título                           |
| `action_`            | Uma ação                            |



