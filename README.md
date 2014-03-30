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

   3). Run database.init.sql if you have not a databse yet (For Asp.Net Identity Framwork 2.0, you should use database.init.2.0.sql)

   3). Update the const in the T4 template for you need, the consts are:
        First you should set IDENTITY_LIB_VERSION to the version of Asp.Net Identity Framwork, currently, only 1 and 2 are supported.

        const int IDENTITY_LIB_VERSION = 2;

	    /**************************************************
	    Const Definitions
	    ***************************************************/
	    //IdentityUser
	    const string USER_TABLE_NAME = "AspNetUsers";
        const string USER_TABLE_ID_COL_NAME = "Id";
	    const string USER_TABLE_ROLE_TABLE_ID_COL_TYPE = "string";
        const int USER_TABLE_ID_COL_MAX_LENGTH = 128;
        const string USER_TABLE_USER_NAME_COL_NAME = "UserName";
	    const int USER_TABLE_USER_NAME_COL_MAX_LENGTH = 256;
        const string USER_TABLE_USER_NAME_INDEX_NAME = "UserNameIndex";
        const string IDENTITY_USER_CLASS_NAME = "IdentityUser";
        var userColMappingDict = new Dictionary<string, Tuple<string, bool, int?>>(); //key is the property name in IdentityUser class, value is the [0]: corresponding column name in table, [1]: is required
        userColMappingDict["PasswordHash"] = new Tuple<string, bool, int?>("PasswordHash", false, null);
        userColMappingDict["SecurityStamp"] = new Tuple<string, bool, int?>("SecurityStamp", false, null);
        switch (IDENTITY_LIB_VERSION)
        {
            case 1:
            {
                userColMappingDict["Discriminator"] = new Tuple<string, bool, int?>("Discriminator", true, null);
                break;
            }
            case 2:
            {
                userColMappingDict["Email"] = new Tuple<string, bool, int?>("Email", false, 256);
                userColMappingDict["EmailConfirmed"] = new Tuple<string, bool, int?>("EmailConfirmed", true, null);
			    userColMappingDict["PhoneNumber"] = new Tuple<string, bool, int?>("PhoneNumber", false, null);
                userColMappingDict["PhoneNumberConfirmed"] = new Tuple<string, bool, int?>("PhoneNumberConfirmed", true, null);
			    userColMappingDict["TwoFactorEnabled"] = new Tuple<string, bool, int?>("TwoFactorEnabled", true, null);
                userColMappingDict["LockoutEndDateUtc"] = new Tuple<string, bool, int?>("LockoutEndDateUtc", false, null);
			    userColMappingDict["LockoutEnabled"] = new Tuple<string, bool, int?>("LockoutEnabled", true, null);
			    userColMappingDict["AccessFailedCount"] = new Tuple<string, bool, int?>("AccessFailedCount", true, null);
                break;
            }
		    default:
		    throw new NotSupportedException("Unknown Identity Version");
        }
	
	    //IdentityUser Mapping
	    const string ROLE_USER_RELATIONSHIP_TABLE_NAME = "AspNetUserRoles";
	    const string ROLE_USER_RELATIONSHIP_TABLE_USER_ID_COL_NAME = "UserId";
	    const string ROLE_USER_RELATIONSHIP_TABLE_ROLE_ID_COL_NAME = "RoleId";
	    //IdentityUserClaim
	    const string USER_CLAIM_TABLE_NAME = "AspNetUserClaims";
        const string USER_CLAIM_TABLE_ID_COL_NAME = "Id";
        const string USER_CLAIM_TABLE_CLAIM_TYPE_COL_NAME = "ClaimType";
        const string USER_CLAIM_TABLE_CLAIM_VALUE_COL_NAME = "ClaimValue";
        const string USER_CLAIM_TABLE_USER_ID_FK_COL_NAME = "UserId";
	    //IdentityUserLogin
	    const string USER_LOGIN_TABLE_NAME = "AspNetUserLogins";
        const string USER_LOGIN_TABLE_LOGIN_PROVIDER_COL_NAME = "LoginProvider";
        const int USER_LOGIN_TABLE_LOGIN_PROVIDER_COL_MAX_LENGTH = 128;
        const string USER_LOGIN_TABLE_PROVIDER_KEY_COL_NAME = "ProviderKey";
        const int USER_LOGIN_TABLE_PROVIDER_KEY_COL_MAX_LENGTH = 128;
        const string USER_LOGIN_TABLE_USER_ID_COL_NAME = "UserId";
	    //IdentityRole
	    const string ROLE_TABLE_NAME = "AspNetRoles";
        const string ROLE_TABLE_ID_COL_NAME = "Id";
	    const int ROLE_TABLE_ID_COL_MAX_LENGTH = 128;
        const string ROLE_TABLE_NAME_COL_NAME = "Name";
	    const int ROLE_TABLE_NAME_COL_MAX_LENGTH = 256;
        const string ROLE_TABLE_ROLE_NAME_INDEX_NAME = "RoleNameIndex";

	    //Path
        const string ENTITY_SUB_NAMESPACE = "Models";
        const string ENTITY_MAPPING_SUB_NAMESPACE = "Models.Mapping";

   4).Run the T4 tempalte

   5. Right click on the project, and selecet Entity Framework/Revise Engineer Code First
