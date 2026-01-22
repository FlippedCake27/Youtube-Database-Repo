
-- 5, Sem 2 Spring, 2024
-- Name: Raymond Chang
-- Email: Raymond.Chang@student.uts.edu.au
-- Student ID: 25618810
--
-- This project is a database application designed to model YouTube channel subscriptions, accounts, playlists, videos, 
-- and interactions such as likes/dislikes and comments. This database assumes that one Youtube account can have multiple channels,
-- to accomodate second channels such as Vsauce 2, Vsauce 3, etc. 
--
-- Quick Overview of each Tables in this Dataset: 
--
-- User Table holds information regarding a Youtube Accoint, from their email, username, contact number, etc
--
-- Comment Table highlights all the comment each user can make, and what video the comment is made in
--
-- Rating Table highlights the like/dislike that a video has, and which users have liked/disliked a video
--
-- Video Table contains all the information for a Youtube video such as its name, when it was created, its description, and views
-- 
-- Channel table contains all the information for a YouTube Channel including the owner of the Youtube channel
--
-- Subscription_Table allows for Channel to subscribe to each other, collecting data on the date of subscription, and who subscribed to who
--
-- Plyalist table contains information regarding a playlist, which Youtube Account made the playlist, and when it was created
--
-- Playlist_Video connects the Playlist Table and Video table, storing data in regards to which video are in which playlist, and what videos are in a playlist
--
-- The inspiration for this project comes from the YouTube subscription system.
-- URL for the web site that inspired this project: https://www.youtube.com


DROP table COMMENT_TABLE CASCADE;
DROP table VIDEO CASCADE;
DROP table USER_TABLE CASCADE;
DROP table RATING CASCADE;
DROP table PLAYLIST CASCADE;
DROP table CHANNEL CASCADE;
DROP table SUBSCRIPTION_TABLE CASCADE;
DROP table PLAYLIST_VIDEO CASCADE;


Create table USER_TABLE
(
	User_ID				integer			NOT NULL,
	Username 			varchar(30)		NOT NULL,
	Email				varchar(50)		NOT NULL,
	Phone_Number		text				,
	User_Created_Date	date		    NOT NULL,

	CONSTRAINT USER_PK PRIMARY KEY (User_ID),

	CONSTRAINT unique_email UNIQUE (Email),

	CONSTRAINT chk_user_id_length CHECK (User_ID BETWEEN 0 AND 999999),
	
	CONSTRAINT chk_user_date CHECK (User_Created_Date >= '2005-04-23')

);

Create table CHANNEL
(
	Channel_ID			integer		NOT NULL,
	User_ID 			integer		NOT NULL,
	Channel_Name		varchar(30)	NOT NULL,
	Channel_Date		date		NOT NULL,
	Subscriber_Count	integer		NOT NULL,

	CONSTRAINT CHANNEL_PK PRIMARY KEY (Channel_ID),

	CONSTRAINT USER_FK_CHANNEL FOREIGN KEY (User_ID) 
				REFERENCES USER_TABLE ON DELETE RESTRICT,

	CONSTRAINT chk_subs CHECK (Subscriber_Count >= 0),

	CONSTRAINT chk_channel_id_length CHECK (Channel_ID BETWEEN 0 AND 999999),

	CONSTRAINT chk_chn_date CHECK (Channel_Date >= '2005-04-23')


);

Create table VIDEO
(
	Video_ID			integer		NOT NULL,
	Channel_ID 			integer		NOT NULL,
	Video_Name			text		NOT NULL,
	Video_Date			date		NOT NULL,
	Description_Info	text				,
	Views_Data        	integer		NOT NULL,

	CONSTRAINT VIDEO_PK PRIMARY KEY (Video_ID),
	
	CONSTRAINT VIDEO_FK FOREIGN KEY (Channel_ID) 
				REFERENCES CHANNEL ON DELETE RESTRICT
	


);

Create table PLAYLIST
(
	Playlist_ID			integer		    NOT NULL,
	User_ID 			integer		    NOT NULL,
	Playlist_Name		varchar(50)		NOT NULL,
	Playlist_Date		date		    NOT NULL,

	CONSTRAINT PLAYLIST_PK PRIMARY KEY (Playlist_ID),

	CONSTRAINT USER_FK_PLAYLIST FOREIGN KEY (User_ID) 
				REFERENCES USER_TABLE ON DELETE CASCADE,

	CONSTRAINT chk_playlist_id_length CHECK (Playlist_ID BETWEEN 0 AND 9999999),

	CONSTRAINT chk_play_date CHECK (Playlist_Date >= '2005-04-23')

);

Create table COMMENT_TABLE
(
	Comment_ID			integer		NOT NULL,
	User_ID 			integer		NOT NULL,
	Video_ID			integer		NOT NULL,
	Comment_Content		text		NOT NULL,
	Comment_Date        date		NOT NULL,

	CONSTRAINT COMMENT_TABLE_PK PRIMARY KEY (Comment_ID),
	
	CONSTRAINT COMMENT_TABLE_USER_FK FOREIGN KEY (User_ID) 
				REFERENCES USER_TABLE ON DELETE CASCADE,
	
	CONSTRAINT COMMENT_TABLE_VIDEO_FK FOREIGN KEY (Video_ID) 
				REFERENCES VIDEO ON DELETE CASCADE,

	CONSTRAINT chk_comment_table_id_length CHECK (Comment_ID BETWEEN 0 AND 9999999),
				
	CONSTRAINT chk_cmt_date CHECK (Comment_Date >= '2005-04-23')
	
);

Create table RATING
(
	Rating_ID			integer		NOT NULL,
	User_ID 			integer		NOT NULL,
	Video_ID			integer		NOT NULL,
	Rating_Type			varchar(7)	NOT NULL,
	Rating_Date		    date		NOT NULL,

	CONSTRAINT RATING_PK PRIMARY KEY (Rating_ID),

	CONSTRAINT USER_FK_RATING FOREIGN KEY (User_ID) 
				REFERENCES USER_TABLE ON DELETE CASCADE,

	CONSTRAINT VIDEO_FK_RATING FOREIGN KEY (Video_ID) 
				REFERENCES VIDEO ON DELETE CASCADE,	

	CONSTRAINT rating_class CHECK (Rating_Type IN (
	
				'Like',
				'Dislike')),

	CONSTRAINT chk_rating_id_length CHECK (Rating_ID BETWEEN 0 AND 9999999),
	
	CONSTRAINT chk_rate_date CHECK (Rating_Date >= '2005-04-23')

);

Create table PLAYLIST_VIDEO
(
	Playlist_ID			integer		NOT NULL,
	Video_ID 			integer		NOT NULL,

	CONSTRAINT PLAYLIST_VIDEO_PK PRIMARY KEY (Playlist_ID,Video_ID ),

	CONSTRAINT PV_PLAYLIST_FK FOREIGN KEY (Playlist_ID) 
				REFERENCES PLAYLIST ON DELETE CASCADE,

    CONSTRAINT PV_VIDEO_FK FOREIGN KEY (Video_ID) 
				REFERENCES VIDEO ON DELETE CASCADE

);


Create table SUBSCRIPTION_TABLE
(
	Subscriber_Channel_ID			integer		NOT NULL,
	Subscribed_Channel_ID           integer		NOT NULL,
	Subscribed_Date			           date		NOT NULL,


	CONSTRAINT SUBSCRIPTION_TABLE_PK PRIMARY KEY (Subscriber_Channel_ID,Subscribed_Channel_ID),

	CONSTRAINT SUBS_USER_FK FOREIGN KEY (Subscriber_Channel_ID) 
				REFERENCES CHANNEL ON DELETE CASCADE,

    CONSTRAINT SUBS_CHANNEL_FK FOREIGN KEY (Subscribed_Channel_ID) 
				REFERENCES CHANNEL ON DELETE CASCADE,

	CONSTRAINT chk_subscription_table_length CHECK (Subscriber_Channel_ID BETWEEN 0 AND 9999999),
    
    CONSTRAINT chk_subscribed_table_length CHECK (Subscribed_Channel_ID BETWEEN 0 AND 9999999),

	CONSTRAINT chk_subs_date CHECK (Subscribed_Date >= '2005-04-23')

);

INSERT INTO USER_TABLE (User_ID, Username, Email, Phone_Number, User_Created_Date) 
VALUES 

    (928572, 'mstennes0', 'lkaines0@stumbleupon.com', '+1 492 365 6282', '2020-09-19'),
    (161691, 'kberetta0', 'mwentworth0@msn.com', null, '2009-11-01'),
    (311963, 'tgeane1', 'lfodden1@wix.com', '+351 882 551 7208', '2012-09-24'),
    (726940, 'mkovalski2', 'tfranzewitch2@ucsd.edu', '+86 994 986 5111', '2022-10-21'),
    (217059, 'zfouracre3', 'csawrey3@go.com', '+593 754 468 4965', '2022-06-19'),
    (408712, 'gormond4', 'bsnufflebottom4@cpanel.net', '+30 620 450 6680', '2015-01-13'),
    (360425, 'tglancy5', 'pdarkin5@ovh.net', '+47 777 338 5397', '2010-01-06'),
    (126709, 'hstate6', 'gpetracek6@hc360.com', '+55 319 968 1446', '2012-01-31'),
    (709178, 'gevelyn7', 'icastiello7@reddit.com', null, '2016-12-22'),
    (759529, 'kborg8', 'frodrigues8@1688.com', null, '2007-05-18'),
    (949260, 'lfletham9', 'tgleasane9@smugmug.com', '+46 530 163 5821', '2016-08-21'),
    (576272, 'kneamesa', 'utynana@so-net.ne.jp', '+1 804 683 7228', '2016-01-07'),
    (100215, 'tburgeb', 'gstonaryb@army.mil', '+30 510 100 3831', '2023-03-03'),
    (440506, 'ktowsiec', 'jdunbobbinc@multiply.com', '+212 414 268 5400', '2011-02-25'),
    (472600, 'atomasicchiod', 'mmalekd@odnoklassniki.ru', '+86 879 307 7757', '2006-11-01'),
    (842151, 'zphilpotte', 'azouche@cargocollective.com', '+46 370 679 1389', '2010-07-05'),
    (990473, 'bvasishchevf', 'dfrantzenif@cloudflare.com', null, '2015-11-18'),
    (408186, 'froafg', 'ohilhouseg@wordpress.com', '+62 816 691 7774', '2017-06-03'),
    (619253, 'cmixerh', 'fperretth@comcast.net', '+974 309 950 1092', '2012-11-28'), 
    (441766, 'tjeskinsi', 'mjozefczaki@joomla.org', '+86 261 550 2757', '2009-05-19'), 
    (862600, 'tbedellj', 'sstainbridgej@oakley.com', '+351 130 756 0587', '2023-10-22'), 
    (622438, 'gbrounek', 'nworgank@dion.ne.jp', '+81 602 997 9599', '2013-09-14'), 
    (786381, 'gjershl', 'mdavidescul@weibo.com', '+963 712 839 2115', '2016-11-05'), 
    (370455, 'scinnamondm', 'phalbeardm@w3.org', '+39 785 412 5540', '2009-06-02'), 
    (762539, 'ldelucian', 'gclapsonn@imageshack.us', '+7 274 454 1863', '2015-08-27'), 
    (674802, 'cmilbourno', 'tpembridgeo@nytimes.com', '+55 212 787 0768', '2024-06-26'), 
    (341651, 'glunckp', 'hconnorp@reverbnation.com', '+86 405 598 8729', '2015-11-25'), 
    (657379, 'elambourneq', 'pslocombeq@dedecms.com', null, '2020-08-20'), 
    (911233, 'cemburyr', 'sboerderr@youtu.be', '+62 176 831 8726', '2022-02-27'), 
    (735918, 'pjerdons', 'fstraffords@tripadvisor.com', '+351 401 934 5465', '2006-01-16'), 
    (578691, 'cduffant', 'asonnenscheint@ezinearticles.com', '+970 180 647 7252', '2006-05-16'), 
    (972448, 'ttitfordu', 'cpargiteru@slate.com', '+351 375 950 4989', '2022-12-02'), 
	(356307, 'cgainev', 'okisbeev@admin.ch', '+63 839 544 3951', '2008-01-30'), 
    (855417, 'aaxcelw', 'iocurranew@kickstarter.com', '+62 356 231 0477', '2007-12-01'), 
    (403365, 'fschoffelx', 'hskeechx@hud.gov', '+381 609 953 1903', '2012-11-29'), 
    (302959, 'dgrisedaley', 'kcardooy@hc360.com', '+62 921 772 0347', '2006-05-08'), 
    (932916, 'sfolkertsz', 'bzealz@usatoday.com', '+48 823 539 3127', '2021-11-06'), 
    (880288, 'nbick10', 'kvanderveldt10@thetimes.co.uk', '+975 170 259 9522', '2007-12-28'), 
    (406620, 'tofaherty11', 'jgurnell11@google.fr', '+62 456 486 7632', '2013-05-26'), 
    (671145, 'ckonmann12', 'cjacob12@behance.net', '+86 404 972 6372', '2015-05-08'), 
    (833412, 'tmixon13', 'lhumpherson13@myspace.com', '+62 589 832 0692', '2007-01-09'), 
    (649700, 'ftargetter14', 'abowlands14@tiny.cc', '+86 634 157 6681', '2021-03-22'), 
    (240890, 'dfrankis15', 'nocklin15@soundcloud.com', '+86 934 324 9619', '2010-07-13'), 
    (772652, 'lthowless16', 'frisom16@blogs.com', '+358 501 585 4533', '2016-08-19'), 
    (210094, 'dpinfold17', 'dilchenko17@mtv.com', '+1 603 246 1065', '2011-05-16'), 
    (246543, 'ejagson18', 'roffell18@amazon.com', '+55 569 793 0083', '2012-03-04'), 
    (555828, 'rmacclancey19', 'gashment19@so-net.ne.jp', '+48 315 455 4536', '2010-08-05'), 
	(938433, 'pwillimot1a', 'dchapellow1a@facebook.com', '+57 126 709 2074', '2014-02-24'), 
    (551624, 'abrilleman1b', 'wsawden1b@acquirethisname.com', '+1 615 174 4036', '2011-09-14'), 
    (504731, 'lpaliser1c', 'northmann1c@ft.com', '+30 586 868 5362', '2020-01-30');
	
INSERT INTO CHANNEL (Channel_ID, User_ID, Channel_Name, Channel_Date, Subscriber_Count) 
VALUES 	
    (102345, 928572, 'Linus Tech Tips', '2010-06-15', 16000000),
    (103987, 161691, 'FitnessBlender', '2012-04-25', 6000000),
    (104567, 311963, 'Binging with Babish', '2015-09-12', 9500000),
    (105123, 726940, 'Kara and Nate', '2018-11-08', 3000000),
    (106789, 217059, 'David Dobrik', '2016-07-23', 18000000),
    (107654, 408712, 'CaseyNeistat', '2019-02-01', 12500000),
    (108098, 360425, 'Markiplier', '2014-08-19', 35000000),
    (109876, 126709, 'Veritasium', '2021-05-10', 13000000),
    (110234, 709178, '5-Minute Crafts', '2017-03-18', 80000000),
    (111345, 759529, 'NPR Music', '2007-05-18', 800000),
    (112456, 949260, 'The Slow Mo Guys', '2011-12-20', 14500000),
    (113567, 576272, 'Good Mythical Morning', '2020-09-30', 18000000),
    (114678, 100215, 'Peter McKinnon', '2013-06-22', 6000000),
    (115789, 440506, 'Art for Kids Hub', '2018-04-13', 6000000),
    (116890, 472600, 'Yoga with Adriene', '2012-02-07', 12000000),
    (117901, 842151, 'Tasty', '2022-07-04', 20000000),
    (118012, 990473, 'Doug DeMuro', '2016-11-29', 5000000),
    (119123, 408186, 'National Geographic', '2019-10-15', 21000000),
    (121345, 441766, 'MKBHD', '2010-01-15', 18000000),
    (122456, 862600, 'Troom Troom', '2017-10-22', 23000000),
    (123567, 622438, 'Japanesepod101', '2013-09-14', 2000000),
    (124678, 786381, 'Simon and Martina', '2016-11-05', 1500000),
    (125789, 370455, 'The School of Life', '2009-06-02', 7000000),
    (126890, 762539, 'Jubilee', '2015-08-27', 7000000),
    (127901, 674802, 'MrBeast Gaming', '2024-06-26', 30000000),
    (128012, 341651, 'Vox', '2015-11-25', 11000000),
    (129123, 657379, 'Vsauce', '2020-08-20', 18000000),
    (130234, 911233, 'Dude Perfect', '2022-02-27', 60000000),
    (131345, 735918, 'TED-Ed', '2006-01-16', 17000000),
    (132456, 578691, 'CGP Grey', '2006-05-16', 5700000),
    (133567, 972448, 'Wendover Productions', '2022-12-02', 4000000),
    (134678, 356307, 'Kurzgesagt – In a Nutshell', '2008-01-30', 20000000),
    (135789, 855417, 'Jacksepticeye', '2007-12-01', 30000000),
    (136890, 657379, 'Vsauce2', '2012-11-29', 4000000),
    (137901, 302959, 'Smosh', '2006-05-08', 25000000),
    (138012, 932916, 'SmarterEveryDay', '2021-11-06', 11000000),
    (139123, 880288, 'PewDiePie', '2007-12-28', 111000000),
    (140234, 406620, 'The Try Guys', '2013-05-26', 8000000),
    (141345, 671145, 'Game Theory', '2015-05-08', 17000000),
    (142456, 833412, 'JennaMarbles', '2007-01-09', 20000000),
    (143567, 649700, 'The Film Theorists', '2021-03-22', 11000000),
    (144678, 240890, 'Science Channel', '2010-07-13', 5000000),
    (145789, 772652, 'Numberphile', '2016-08-19', 4000000),
    (146890, 210094, 'Brave Wilderness', '2011-05-16', 20000000),
    (147901, 246543, 'Hot Ones', '2012-03-04', 12000000),
    (148012, 555828, 'PBS Eons', '2010-08-05', 3000000),
    (149123, 938433, 'DIY Perks', '2014-02-24', 4200000),
    (150234, 551624, 'Every Frame a Painting', '2011-09-14', 1800000),
    (151345, 504731, 'MinutePhysics', '2020-01-30', 5000000),
    (152456, 949260, 'The Slow Mo Guys 2', '2010-10-20', 14500000),
    (153567, 880288, 'Pewdiepie 2', '2012-02-19', 1800000),
	(154789, 619253, 'Life of Boris', '2014-04-23', 4000000),
	(160345, 619253, 'Life of Boris 2', '2020-12-08', 300000),
	(159234, 408712, 'Neistat Vlogs', '2022-02-05', 1000000),
	(160346, 360425, 'Markiplier Shorts', '2015-12-10', 7000000),
	(162567, 709178, 'Crafty Crafts', '2018-03-29', 75000000),
	(164789, 949260, 'Slow Mo HQ', '2019-11-30', 17000000);

INSERT INTO VIDEO (Video_ID, Channel_ID, Video_Name, Video_Date, Description_Info, Views_Data) 
VALUES 
    (2021001, 102345, 'How to Build a PC - Step by Step', '2021-03-15', 'A complete guide to building a computer from scratch.', 1500000),
    (2020002, 103987, '15 Minute Full Body Workout', '2020-10-10', 'A quick and effective workout to tone your body.', 300000),
    (2022003, 104567, 'The Perfect Pasta Carbonara', '2022-02-28', 'Learn to cook authentic Italian pasta carbonara.', 800000),
    (2019004, 105123, 'Exploring the Himalayas', '2019-06-14', 'A travel vlog documenting a trip to the Himalayas.', 450000),
    (2018005, 106789, 'David Dobrik - Best Pranks Compilation', '2018-12-24', 'Compilation of the best pranks pulled by David Dobrik.', 7500000),
    (2022006, 107654, 'How to Edit Videos like Casey Neistat', '2022-01-05', 'A tutorial on editing videos with a cinematic style.', 2200000),
    (2020007, 108098, 'Markiplier Tries VR Horror Games', '2020-08-10', 'Markiplier plays various VR horror games and reacts to them.', 5000000),
    (2021008, 109876, 'Veritasium Explains Quantum Mechanics', '2021-04-19', 'An in-depth explanation of quantum mechanics concepts.', 2500000),
    (2019009, 110234, 'DIY Crafts with 5-Minute Crafts', '2019-07-22', 'Easy DIY projects that you can complete in just 5 minutes.', 60000000),
    (2023010, 111345, 'Tiny Desk Concert - NPR Music', '2023-06-01', 'Live concert performance at the NPR Music Tiny Desk.', 700000),
    (2017011, 112456, 'Slow Motion Water Balloon Pop', '2017-05-18', 'Watch a water balloon pop in slow motion.', 20000000),
    (2018012, 113567, 'Good Mythical Morning - Weird Food Combos', '2018-09-09', 'Rhett and Link try weird food combinations.', 3000000),
    (2020013, 114678, 'Peter McKinnon - Photography Tips', '2020-11-11', 'Peter shares photography techniques for beginners.', 1000000),
    (2021014, 115789, 'Drawing with Art for Kids Hub', '2021-02-27', 'A fun and easy drawing tutorial for kids.', 850000),
    (2019015, 116890, 'Yoga for Beginners with Adriene', '2019-01-10', 'A gentle yoga session for beginners.', 4000000),
    (2022016, 117901, 'Tasty - How to Make a Perfect Burger', '2022-08-04', 'Tasty shows you how to cook the perfect burger.', 17000000),
    (2021017, 118012, 'Doug DeMuro Reviews the Tesla Model S', '2021-09-20', 'Doug reviews the 2021 Tesla Model S.', 2700000),
    (2020018, 119123, 'National Geographic - Wildlife Documentary', '2020-07-14', 'A documentary on the wildlife of the Amazon rainforest.', 3100000),
    (2021019, 121345, 'MKBHD - iPhone 13 Review', '2021-10-12', 'Marques reviews the latest iPhone 13.', 8000000),
    (2018020, 122456, 'Troom Troom Pranks', '2018-11-30', 'Prank ideas from Troom Troom.', 23000000),
    (2018021, 104567, 'Binging with Babish: The Moistmaker from Friends', '2018-12-10', 'Learn how to recreate the famous Moistmaker sandwich from Friends.', 4500000),
    (2019022, 104567, 'Binging with Babish: Ratatouille', '2019-05-07', 'Cooking the iconic ratatouille from the movie Ratatouille.', 5200000),
    (2021023, 139123, 'PewDiePie Reacts to Memes', '2021-02-14', 'PewDiePie reacts to the latest memes on the internet.', 35000000),
    (2020024, 139123, 'PewDiePie Tries Swedish Snacks', '2020-09-21', 'PewDiePie tastes and reviews various Swedish snacks.', 27000000),
    (2020025, 135789, 'Jacksepticeye Plays Among Us with Friends', '2020-10-05', 'Jacksepticeye plays the popular game Among Us with his friends.', 18000000),
    (2019026, 135789, 'Jacksepticeye’s Funniest Moments Compilation', '2019-12-25', 'A compilation of the funniest moments from Jacksepticeye’s videos.', 22000000),
    (2019027, 153567, 'PewDiePie 2: Minecraft Adventures', '2019-08-01', 'PewDiePie explores a new Minecraft world and builds his first house.', 1500000),
    (2020028, 153567, 'PewDiePie 2: Reviewing Fan Art', '2020-03-15', 'PewDiePie reacts to fan art sent in by his community.', 2000000),
    (2020029, 160345, 'Life of Boris 2: How to Make Russian Pancakes', '2021-01-05', 'Boris shares his family recipe for Russian pancakes (blini).', 500000),
    (2020030, 160345, 'Life of Boris 2: Slavic Car Review', '2020-11-18', 'Boris reviews a classic Soviet-era car, the Lada.', 350000),
    (2018031, 160346, 'Markiplier Shorts: Try Not to Laugh Challenge', '2018-05-12', 'Markiplier attempts a hilarious Try Not to Laugh challenge.', 6500000),
    (2019032, 160346, 'Markiplier Shorts: Reading Funny Comments', '2019-08-22', 'Markiplier reads and reacts to funny comments from his viewers.', 7000000);

INSERT INTO SUBSCRIPTION_TABLE (Subscriber_Channel_ID, Subscribed_Channel_ID, Subscribed_Date)
VALUES 
    (102345, 103987, '2024-01-05'), -- Linus Tech Tips subscribes to FitnessBlender
    (104567, 105123, '2024-02-18'), -- Binging with Babish subscribes to Kara and Nate
    (106789, 108098, '2023-12-20'), -- David Dobrik subscribes to Markiplier
    (107654, 110234, '2024-03-12'), -- CaseyNeistat subscribes to 5-Minute Crafts
    (109876, 112456, '2024-04-07'), -- Veritasium subscribes to The Slow Mo Guys
    (113567, 105123, '2024-05-15'), -- Good Mythical Morning subscribes to Kara and Nate
    (114678, 119123, '2024-06-03'), -- Peter McKinnon subscribes to National Geographic
    (115789, 116890, '2024-07-21'), -- Art for Kids Hub subscribes to Yoga with Adriene
    (121345, 122456, '2024-08-14'), -- MKBHD subscribes to Troom Troom
    (126890, 124678, '2024-09-10'), -- Jubilee subscribes to Simon and Martina
    (127901, 135789, '2024-10-01'), -- MrBeast Gaming subscribes to Jacksepticeye
    (128012, 132456, '2024-02-24'), -- Vox subscribes to CGP Grey
    (129123, 133567, '2024-03-13'), -- Vsauce subscribes to Wendover Productions
    (130234, 134678, '2024-04-27'), -- Dude Perfect subscribes to Kurzgesagt – In a Nutshell
    (136890, 138012, '2024-05-19'), -- Vsauce2 subscribes to SmarterEveryDay
    (139123, 141345, '2024-06-22'), -- PewDiePie subscribes to Game Theory
    (142456, 143567, '2024-07-09'), -- JennaMarbles subscribes to The Film Theorists
    (144678, 146890, '2024-08-02'), -- Science Channel subscribes to Brave Wilderness
    (147901, 148012, '2024-09-16'), -- Hot Ones subscribes to PBS Eons
    (102345, 146890, '2024-01-05'), -- Linus Tech Tips subscribes to Brave Wilderness
    (146890, 102345, '2024-01-05'), -- Brave Wilderness subscribes to Linus Tech Tips
    (149123, 150234, '2024-10-07'); -- DIY Perks subscribes to Every Frame a Painting

INSERT INTO COMMENT_TABLE (Comment_ID, User_ID, Video_ID, Comment_Content, Comment_Date)
VALUES
    (1020, 928572, 2021001, 'This video was really helpful for building my first PC, thanks!', '2021-06-25'),
    (1021, 161691, 2020002, 'Great workout! I feel the burn already.', '2021-03-15'),
    (1022, 311963, 2022003, 'Finally! A carbonara recipe that actually tastes authentic!', '2022-03-01'),
    (1023, 126709, 2019004, 'The Himalayas look stunning! I wish I could visit someday.', '2019-06-20'),
    (1024, 759529, 2018005, 'These pranks are hilarious! Please do more!', '2018-12-30'),
    (1025, 408712, 2022006, 'The editing style is truly unique. Loved the tutorial!', '2022-01-10'),
    (1026, 360425, 2020007, 'Markiplier’s reactions are priceless! Can’t stop laughing.', '2020-08-15'),
    (1027, 709178, 2019009, 'These DIY crafts are perfect for quick projects. Thanks!', '2019-08-01'),
    (1028, 576272, 2017011, 'The slow-motion effect on the water balloon was amazing!', '2017-06-10'),
    (1029, 576272, 2018012, 'Weird food combos are always a blast to watch! Try more.', '2018-09-12'),
    (1030, 990473, 2022016, 'Best burger tutorial ever! Now I’m hungry.', '2022-08-06'),
    (1031, 862600, 2021017, 'I love Doug’s car reviews. The Tesla is awesome!', '2021-10-01'),
    (1032, 674802, 2018021, 'The Moistmaker is iconic! Perfect recreation.', '2018-12-15'),
    (1033, 855417, 2020025, 'Among Us never gets old. Jack makes it even funnier.', '2020-10-10'),
    (1034, 880288, 2019027, 'PewDiePie’s Minecraft adventures are always entertaining.', '2019-08-05'),
    (1035, 311963, 2019027, 'This Minecraft episode was hilarious! Keep them coming.', '2019-09-05'),
    (1036, 217059, 2019027, 'The builds are getting better and better!', '2019-08-20'),
    (1037, 126709, 2021019, 'Loved the iPhone 13 review. Very detailed and informative.', '2021-11-02'),
    (1038, 126709, 2022003, 'The pasta carbonara looks delicious! Can’t wait to try it.', '2022-03-18');



INSERT INTO RATING (Rating_ID, User_ID, Video_ID, Rating_Type, Rating_Date) 
VALUES
    (101001, 928572, 2021001, 'Like', '2021-06-26'),
    (101002, 161691, 2020002, 'Like', '2020-11-11'),
    (101003, 311963, 2022003, 'Dislike', '2022-03-15'),
    (101004, 126709, 2019004, 'Like', '2019-07-02'),
    (101005, 759529, 2018005, 'Like', '2019-01-01'),
    (101006, 408712, 2022006, 'Dislike', '2022-01-20'),
    (101007, 360425, 2020007, 'Like', '2020-08-18'),
    (101008, 709178, 2019009, 'Dislike', '2019-08-08'),
    (101009, 576272, 2018012, 'Like', '2018-09-20'),
    (101010, 990473, 2022016, 'Like', '2022-08-12'),
    (101011, 990473, 2021001, 'Like', '2021-06-26'),
    (101012, 408712, 2019009, 'Dislike', '2019-08-08');


INSERT INTO PLAYLIST (Playlist_ID, User_ID, Playlist_Name, Playlist_Date)
VALUES 
    (110001, 928572, 'Tech Tutorials', '2021-06-15'),
    (120202, 161691, 'Workout Routines', '2020-09-20'),
    (130503, 311963, 'Cooking with Babish', '2022-01-05'),
    (140304, 726940, 'Travel Vlogs', '2019-03-12'),
    (150805, 217059, 'Comedy Sketches', '2018-11-22'),
    (160706, 408712, 'Film Analysis', '2022-02-15'),
    (170607, 360425, 'Gaming Highlights', '2021-10-08'),
    (180908, 126709, 'Science Explained', '2020-05-25'),
    (190109, 709178, 'DIY Projects', '2018-07-30'),
    (160711, 408712, 'Movie OSTs', '2022-02-19'),
    (150815, 217059, 'Anime Songs', '2018-12-25'),
    (150835, 217059, 'Minecraft', '2018-12-25'),
    (150825, 759529, 'Minecraft Playlist', '2018-12-25'),
    (200410, 759529, 'Music Covers', '2019-12-05');


INSERT INTO PLAYLIST_VIDEO (Playlist_ID, Video_ID)
VALUES
    (110001, 2021001), -- Tech Tutorials includes "How to Build a PC - Step by Step"
    (110001, 2021008), -- Tech Tutorials includes "Veritasium Explains Quantum Mechanics"
    (120202, 2020002), -- Workout Routines includes "15 Minute Full Body Workout"
    (120202, 2018005), -- Workout Routines includes "David Dobrik - Best Pranks Compilation"
    (130503, 2022003), -- Cooking with Babish includes "The Perfect Pasta Carbonara"
    (130503, 2018021), -- Cooking with Babish includes "Binging with Babish: The Moistmaker from Friends"
    (140304, 2019004), -- Travel Vlogs includes "Exploring the Himalayas"
    (140304, 2018031), -- Travel Vlogs includes "Markiplier Shorts: Try Not to Laugh Challenge"
    (150805, 2021023), -- Comedy Sketches includes "PewDiePie Reacts to Memes"
    (160706, 2018012), -- Film Analysis includes "Good Mythical Morning - Weird Food Combos"
    (150835, 2019027), -- Minecraft incldes "PewDiePie explores a new Minecraft world and builds his first house."
    (150825, 2019027); -- Minecraft Playlist incldes "PewDiePie explores a new Minecraft world and builds his first house."

-- View Table for Checking all the Videos that Channels have 

CREATE VIEW Channel_Video_Overview AS
SELECT C.Channel_ID, C.Channel_Name, V.Video_ID, V.Video_Name, V.Views_Data
FROM CHANNEL C, Video V
WHERE C.Channel_ID = V.Channel_ID
ORDER BY C.Channel_Name;
