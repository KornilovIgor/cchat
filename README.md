# CChat: Консольный чат

Данный простейший консольный чат содержит три класса:
Chat - главный управляющий класс, в котором описаны все методы управления
Message - класс сообщений, содержащий в себе два указателя на получателя и отправителя текста сообщения (std::string text).
User - класс, описывающий пользователя - имя, логин, пароль.

Класс User:
_login - логин пользователя (строка);
_password - пароль пользователя (строка) (для удобства тестов минимальная длина пароля - 1 символ);
_name - имя пользователя (строка);

Класс Message:
Переменные _to и _from - указатели shared_ptr на User. Реализовано через указатели чтобы пользователи могли менять логин и пароль и эти изменения не требовали изменения сообщений.
std::string _text - текст сообщения.

Класс Chat:
_users - список пользователей (вектор указателей на объект User). Т.к. одним указателем могут одновременно владеть и _users и _currentUser, выбран shared_ptr.

_messages - список сообщений (вектор указателей на объект Message). Владение всеми указателями у _message, поэтому для оптимизации выбран unique_ptr.

_currentUser - указатель на пользователя, который является текущим активным.

menuStart - вывод стартового меню.
menuMain - вывод пользовательского меню.

getUserByLogin - получить пользователя по логину. Проход по всем пользователям и если встречается c указанным логином - возвращает указатель на найденного пользователя

getUserByIndex - получить пользователя по индексу. Возвращает указатель на пользователя по индексу. Возможна исключительная ситуация - out_of_range (ситуация искусственно не исключена другими методами ради демонстрации работы исключений)
showUserByIndex - (ещё один "лишний" метод для демонстрации исключений)

addUser - добавить пользователя. Создаём по полученным данным (логин, пароль, имя) shared пользователя и пушим его в список пользователей (vector<User>)

addMessage - добавить сообщение. Создаём по полученным данным (указатель на получателя, указатель на отправителя, текст сообщения) unique сообщение и пушим его в список сообщений (vector<Message>)

signUp - регистрация. Запрашиваем ввод логина. Проверяем валидность логина. Если ошибка - запрашиваем нужна ли ещё попытка. Аналогично с паролем. После ввода пароля требуется повторить ввод пароля - они должны совпадать. Иначе либо повторяется ввод, либо выходим из регистрации. Имя вводится без каких-либо проверок.

signIn - авторизация. Просим ввести логин (без проверок валидности, они были при регистрации). Если введенный логин не соответствует ни одному пользователю - выходим из авторизации. Запрашиваем пароль. Если пароль соответствует пользователю - переходим в главное меню. Иначе выход из меню авторизации.

showMessages - вывести сообщения. Вызывает метод printMessage для всех сообщений, имеющих отношение к текущему пользователю. Либо он отправитель, либо он получатель, либо сообщение предназначено для всех.

printMessage - вывести сообщение в консоль. Если пользователь отправитель или получатель - имя заменяется на "me". (в дальнейшем можно дополнительно выделить своим цветом). Если получатель - nullptr - вместо логина выводится "to all"

sendPrivateMessage - отправить личное сообщение конкретному пользователю. Система запрашивает логин получателя -> если найден пользователь с таким логин: система запрашивает текст сообщения. Вызывает метод addMessage и передаётся _currentUser, получатель и текст сообщения.

sendPublicMessage - отправить сообщение всем. Запрашивается текст сообщения. Вызывается метод addMessage и передаётся в него текущий пользователь как отправитель, nullptr - как получатель и текст сообщения.

printStartMenu, printUserMenu - вывод соответствующих названию меню на экран.

inputMenu - запрос ввода номера меню с проверкой ввода. Проверяется соответствие int. На входе получается количество пунктов меню, не считая выхода. Если вводится значение меньше 0 или меньше количества пунктов меню - выводится соответствующая ошибка и повтор ввода.

isValidLogin - допустимый ли логин. Проверяется длина и использование допустимых символов 0..9, a..z, A..Z.

isValidPassword - допустимый ли пароль. Аналоично логину.

repeat - повторить. Вызывается при каких-либо ошибках ввода. Система запрашивает Повторить или Вернуться назад.
