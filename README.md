AspNet.Identity.EntityFramework
===============================

1. AspNet.Identity.EntityFramework
   With the new release of MVC 5, Microsoft add AspNet.Identity Framwrork, but the AspNet.Identity.EntityFramework only support Code First Pattern, while for Databas/Model First Pattern, it is hard to code all the class by hand.
   so we add the AspNet.Identity.EntityFramework.tt to support this pattern. You can create you database first and then use this T4 template to automaticly generate the AspNet.Identity compatible classes.
   There are some limitions:
   1). The table for IdentityRole, IdentityUserClaim, IdentityUserLogin and IdentityUserRole should be same as default schema from AspNet.Identity.EntityFramework (you can use database.init.sql to create the basic schema if you don't konw the schema of the AspNet.Identity.EntityFramework default database).
   2). you can add more columns to the user table but don't remove any default columns from the user table.

2. Steps:
   1). Install EF Power Tools (http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)
   2). Add the T4 template from the NuGet
   3). Run database.init.sql if you have not a databse yet
   3). Update the const in the T4 template for you need, he consts are:\
      const string USER_TABLE_NAME = "User";
      const string USER_TABLE_ID_COL_NAME = "Id";
      const string USER_TABLE_USER_NAME_COL_NAME = "UserName";
	    const string USER_TABLE_PASSWORDHASH_COL_NAME = "PasswordHash";
	    const string USER_TABLE_SECURITYSTAMP_COL_NAME = "SecurityStamp";
	    const string USER_TABLE_DISCRIMINATOR_COL_NAME = "Discriminator";
      const string IDENTITY_USER_CLASS_NAME = "IdentityUser";
	    //IdentityUser Mapping
	    const string ROLE_USER_RELATIONSHIP_TABLE_NAME = "UserRole";
	    const string ROLE_USER_RELATIONSHIP_TABLE_USER_ID_COL_NAME = "UserId";
	    const string ROLE_USER_RELATIONSHIP_TABLE_ROLE_ID_COL_NAME = "RoleId";
	    //IdentityUserClaim
	    const string USER_CLAIM_TABLE_NAME = "UserClaim";
      const string USER_CLAIM_TABLE_ID_COL_NAME = "Id";
      const string USER_CLAIM_TABLE_CLAIM_TYPE_COL_NAME = "ClaimType";
      const string USER_CLAIM_TABLE_CLAIM_VALUE_COL_NAME = "ClaimValue";
      const string USER_CLAIM_TABLE_USER_ID_FK_COL_NAME = "UserId";
	    //IdentityUserLogin
	    const string USER_LOGIN_TABLE_NAME = "UserLogin";
      const string USER_LOGIN_TABLE_LOGIN_PROVIDER_COL_NAME = "LoginProvider";
      const string USER_LOGIN_TABLE_PROVIDER_KEY_COL_NAME = "ProviderKey";
      const string USER_LOGIN_TABLE_USER_ID_COL_NAME = "UserId";
	    //IdentityRole
	    const string ROLE_TABLE_NAME = "Role";
      const string ROLE_TABLE_ID_COL_NAME = "Id";
      const string ROLE_TABLE_NAME_COL_NAME = "Name";

	    //Path
      const string ENTITY_SUB_NAMESPACE = "Models";
      const string ENTITY_MAPPING_SUB_NAMESPACE = "Models.Mapping";
   4).Run the T4 tempalte
   5. Right click on the project, and selecet Entity Framework/Revise Engineer Code First
