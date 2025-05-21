### Configuration

In vars, create :
- **wheel_users** list of new users which will belong to wheel group, so being able to `sudo`
- **users** list of users not belonging to wheel.

> [!NOTE]
> The initial password is the account name (i.e. for the user "laurent", the password will
> be "laurent" as well ... and will have to be reset at the initial login).
