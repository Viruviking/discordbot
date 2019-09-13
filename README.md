# Бот Discord для Викингов Вирумаа
Nodejs приложение для управления сообществами Sunfox Team. Бот сосздавался под определенный чат, и к сожалению, не является универсальным. Однако, Вы можете создать бота для свего сообщества на его основе. 

Бот пока еще молодой и умеет не многое:
* читать массив с данными предстоящих мероприятий и показывать их список по команде
* по команде добавлять новые достижения в базу данных (доступно администраторам)
* автоматически добавляет новоприбывших участников в базу данных
* по команде фиксирует факт получения той или иной ачивки (доступно администраторам)
* по команде показывает профиль участника, включая уровень и список полученных достижений (а также тех, которые требуется получить для достижениян ового уровня)

TODO:
* научить бота обрабатывать стандартные языки разметки календарей

## Установка
Технические требования:
* Node.JS >= 10.16.3
* MySQL >= 5.5

Подключитесь к своему серверу по протоколу SSH, разверние репозиторий в рабочую директорию и инициализируйте Node.JS в ней же:
```bash
git clone https://github.com/Viruviking/discordbot.git bot.discord
cd bot.discord/
npm init -y
npm install --save discord.js
```
Далее скорректируйте файл config.json по шаблону:
```json
{
        "token": "[your Discord bot token here]",
        "log_channel": "[read-onle log channel title here]",
        "admin_roles": { [Moderator & Admin roles title here] },
        "db_config": {"host":"localhost", "dbname":"[database title]", "dbuser":"[database username]", "dbpass":"[database user password]" }
}
```
Создайте базу данных в соотвествии с данными подключения, указанными в файле конфигурации. Для развертывания БД выполните следующий код MySQL:
```mysql
SET NAMES utf8;

CREATE DATABASE `sfx_drdbot` /*!40100 DEFAULT CHARACTER SET utf8 */;
USE `sfx_drdbot`;

DROP TABLE IF EXISTS `drd_achievements`;
CREATE TABLE `drd_achievements` (
  `id` smallint(6) NOT NULL AUTO_INCREMENT,
  `title` varchar(55) NOT NULL,
  `description` varchar(512) NOT NULL,
  `level_id` smallint(6) NOT NULL,
  `community` varchar(55) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

CREATE TABLE `drd_levels` (
  `id` smallint(6) NOT NULL AUTO_INCREMENT,
  `level` smallint(6) NOT NULL,
  `title` varchar(55) NOT NULL,
  `description` varchar(512) NOT NULL,
  `community` varchar(55) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

DROP TABLE IF EXISTS `drd_users`;
CREATE TABLE `drd_users` (
  `id` smallint(6) NOT NULL AUTO_INCREMENT,
  `level` smallint(6) NOT NULL,
  `community` varchar(55) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

DROP TABLE IF EXISTS `drd_usr_ach`;
CREATE TABLE `drd_usr_ach` (
  `id` smallint(6) NOT NULL AUTO_INCREMENT,
  `date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `ach_id` smallint(6) NOT NULL,
  `user_id` smallint(6) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```
Внимание! База данных не содержит материалов (ачивок и уровней системы достижений).

Для запуска бота выполните команду из рабочей директории:
```bash
node bot.js
```