� �������� ���� ������ ������������ ����������� ��.
�� �������� ��� �������:
```
users (id, login)
teams (id, name)
users_teams (user_id, team_id)
```
���������� ������� �� ������ ����� ��������� ���������� (��. ��� ������ Demo)
� ����� ������� ������� sql � ��������� � ��� ������ �������� ���� ������ `db-create.sql`

������� � ����������� ���� ����� �������, ����� ��� ������� ������ Demo 
������������ ��������������� ����������������.
--------------------------------------------------
�����: `com.epam.rd.java.basic.topic07.task03`
������: 
Demo - �������� ������������ �����������, �������� ��� ���������.
��������� ������ ��. �����������.
--------------------------------------------------
���������� ������ Demo:
```
public class Demo {

	public static void main(String[] args) throws DBException {
		// users  ==> [ivanov, petrov]
		// teams  ==> [teamA teamB teamC]
		// teamA contains the following users: ivanov, petrov 
		
		DBManager dbManager = DBManager.getInstance();
		User userPetrov = dbManager.getUser("petrov");
		System.out.println((dbManager.getUserTeams(userPetrov)));
		// [teamA]

		Team teamA = dbManager.getTeam("teamA");
		Team teamC = dbManager.getTeam("teamC");
		System.out.println((dbManager.getTeamUsers(teamA)));
		// [ivanov, petrov]
		
		// on delete cascade!
		dbManager.deleteTeam(teamA);
		teamC.setName("teamX");
		dbManager.updateTeam(teamC);
		System.out.println((dbManager.findAllTeams()));
		// teams ==> [teamB, teamX]
		
		for (Team team : dbManager.findAllTeams()) {
			dbManager.deleteTeam(team);
		}

		dbManager.insertTeam(Team.createTeam("teamB"));
		System.out.println((dbManager.findAllTeams()));
		// teams ==> [teamB]        
		
		User userIvanov = dbManager.getUser("ivanov");
		System.out.println((dbManager.getUserTeams(userIvanov)));
		// teams ==> []
	}
}

```
(1) ����� DBManager#`setTeamsForUser(User, Team...)` ����������� � ������� 
����������: ���������� ������ ������� ������ ������������ ����� ��������� ���� 
��� ������, ���������� � ������ ����������, ���� �� ���� �� ���.

���� ����� ����� ������ ���: `setTeamsForUser(user, teamA, teamB, teamC)`,
�� � ������� ������ `users_teams` ������ ������ ���� ��������� ��������������� 
� ������� ��������� ����� � ������ ���������� ����� �������:
```
user_id, teamA_id
user_id, teamB_id 
user_id, teamC_id
```
���� ��������� ������ �� ����� ���� ���������, �� ������ ��� ����� �� ������ �������
� ���� ������.

(2) ����� DBManager#`getUserTeams` ������ ���������� ������ `List<Team>`.

(3) ����� DBManager#`deleteTeam` ������� ������ �� �����.
��� �������� ������ �� ������� users_teams ����� ������ ���� �������.
��������� ����������� � ������� ���������� ����������� ��������� �����������:
`ON DELETE CASCADE`.

(4) ����� DBManager#`updateTeam(Team team)` ��������� ������ � ���� ������.

(5) ����� DBManager#`insertUser` ������ �������������� ���� id ������� User.

(6) ����� DBManager#`findAllUsers` ���������� ������ `java.util.List<User>`.

(7) ����� DBManager#`insertTeam` ������ �������������� ���� id ������� Team.

(8) ����� DBManager#`findAllTeams` ���������� ������ `java.util.List<Team>`.

##### ���������.
 ����� User ������ ���������:
- ����� `getLogin()`, ������� ���������� ����� ������������;
- ����� `toString()`, ������� ���������� ����� ������������;
- ���������� ������ `equals(Object obj)`, �������� ������� ��� ������� User 
����� ����� � ������ �����, ����� ��� ����� ���� �����;
- ����������� ����� `createUser(String login)`, ������� ������� ������ User �� 
������ (������������� ����� 0).

����� Team ������ ���������:
- ����� `getName()`, ������� ���������� �������� ������;
- ����� `toString()`, ������� ���������� �������� ������;
- ���������� ������ `equals(Object obj)`, �������� ������� ��� ������� Team 
����� ����� � ������ �����, ����� ��� ����� ���� ��������.
- ����������� ����� `createTeam(String name)`, ������� ������� ������ Team �� 
����� (������������� ����� 0).
