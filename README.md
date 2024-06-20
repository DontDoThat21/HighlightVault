Highlights Vault is a .NET Core MVC application that stores descriptions, names, dates, a picture, and a "highlight" video clip per row.
The application has two "modes" powered by simple Password entry (SQL table entry with a password. Should consider changing this ESPECIALLY if/when not using this locally.)

Supports Steam Api adding and automatically saving (profile picture to the database directly as bytes/blob object) Steam User's datas (name, picture) to the SQL Server DB.
https://steamcommunity.com/dev/apikey

Designed to use Entity Framework with a SQL Server.
The DDL for the tables can all be found below!
The database name I used was "HighlightsVault".

The biggest regret I have with this app, is writing a "AddHighlight" (controller IActionResult method) and using it for BOTH Singular, and Multiple and/or Multiple Group entries.
It's not too awful but, I probably should have seperated these methods. Maybe one day!

Thanks for viewing.

---- Main table
USE [HighlightsVault]
GO

/****** Object:  Table [dbo].[HighlightsVault]    Script Date: 6/20/2024 6:49:20 PM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[HighlightsVault](
	[ID] [int] IDENTITY(1,1) NOT NULL,
	[steamID] [nvarchar](50) NOT NULL,
	[GroupId] [int] NULL,
	[HighlightPersonName] [nvarchar](100) NULL,
	[UserDescription] [nvarchar](510) NULL,
	[ProfileUrl] [nvarchar](255) NULL,
	[ProfilePictureUrl] [nvarchar](255) NULL,
	[ProfilePicture] [varbinary](max) NULL,
	[HighlightDate] [datetime] NOT NULL,
	[CreatedAt] [datetime] NOT NULL,
	[Clip] [varbinary](max) NULL,
 CONSTRAINT [PK__HighlightVa__3214EC2721277B20] PRIMARY KEY CLUSTERED 
(
	[ID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO

ALTER TABLE [dbo].[HighlightsVault]  WITH CHECK ADD  CONSTRAINT [FK_HighlightsVault_Groups] FOREIGN KEY([GroupId])
REFERENCES [dbo].[HighlightsVaultGroups] ([GroupId])
GO

ALTER TABLE [dbo].[HighlightsVault] CHECK CONSTRAINT [FK_HighlightsVault_Groups]
GO


---- Highlight Vaults foreign key table containing group definitions when adding multiple users at once.
USE [HighlightsVault]
GO

/****** Object:  Table [dbo].[HighlightsVaultGroups]    Script Date: 6/20/2024 6:50:18 PM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[HighlightsVaultGroups](
	[GroupId] [int] IDENTITY(1,1) NOT NULL,
	[CreatedAt] [datetime] NOT NULL,
 CONSTRAINT [PK__HighlightVaultsGro__149AF36ABF705B5F] PRIMARY KEY CLUSTERED 
(
	[GroupId] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

---- Fairly primitive table that stores simple passwords that trigger different views for the main page text box entry
USE [HighlightsVault]
GO

/****** Object:  Table [dbo].[Passwords]    Script Date: 6/20/2024 6:50:56 PM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[Passwords](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[PasswordValue] [nvarchar](100) NOT NULL,
PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO


