_G.love = require("love")
_G.suit = require "libraries.suit"
local cron = require "libraries.cron"
local inspect = require "libraries.inspect"
local lunajson_encode = require('libraries.lunajson.encoder')
local lunajson_decode = require('libraries.lunajson.decoder')
local lunajson_sax = require('libraries.lunajson.sax')
local decode = lunajson_decode()
local encode = lunajson_encode()
local seconds = 0
local timer = cron.every(1, function() seconds = seconds - 1 end)
local input = {text = ""}
local slider = {min = 0, value = 50, max = 100}
local chk = {}
local acceptedUsername = true
local acceptedEmail = true
local acceptedPassword = true
local selectLanguage = "English"
local musicSelected = true
local soundefSelected = true
local veselect = 1
local item = 1
local itemName = ""
local acceptedTime = true
local totalTime = 0
local timeAdded = 0
local totalItems = 0
local spot1place = false
local spot2place = false
local spot3place = false

-- storage used for items
local storage = {
    spaces = {
        slot1 = "",
        slot2 = "",
        slot3 = "",
    },
    places = {
        place1 = "",
        place2 = "",
        place3 = "",
    }
}

-- font styles stored in a table
local font = {
    type = {
        fontlarge = love.graphics.newFont("fonts/Ubuntu/Ubuntu-BoldItalic.ttf", 30),
        fontmain = love.graphics.newFont("fonts/Ubuntu/Ubuntu-BoldItalic.ttf", 20),
        fontsmall = love.graphics.newFont("fonts/Ubuntu/Ubuntu-LightItalic.ttf", 13),
        font2large = love.graphics.newFont("fonts/georgiaz.ttf", 30),
        font2main = love.graphics.newFont("fonts/georgia.ttf", 20),
        font2small = love.graphics.newFont("fonts/georgiai.ttf", 13),
        font3small = love.graphics.newFont("fonts/Ubuntu/Ubuntu-BoldItalic.ttf", 15),
        timerfont = love.graphics.newFont("fonts/Ubuntu/Ubuntu-BoldItalic.ttf", 50),
    }
}

-- app states stored in a table
local app = {
    state = {
        start = false,
        signup = false,
        login = true,
        userName = true,
        userPassword = false,
        userEmail = false,
        menu = false,
        popOut = false,
        timer = false,
        ve = false,
        settings = false,
        account = false,
        display = false,
        chpassword = false,
        currentp = false,
        newp = false,
        chemail = false,
        currente = false,
        newe = false,
        chpfp = false,
    }
}

function love.load()
    -- sets background colour
    love.graphics.setBackgroundColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
    -- importing music/ambience
    _G.cafemusic = love.audio.newSource("music/cafeambience.mp3", "stream")
    _G.librarymusic = love.audio.newSource("music/libraryambience.mp3", "stream")
    _G.forestmusic = love.audio.newSource("music/forestambience.mp3", "stream")
    _G.citymusic = love.audio.newSource("music/cityambience.mp3", "stream")
    -- looping the music/ambience
    cafemusic:setLooping(true)
    librarymusic:setLooping(true)
    forestmusic:setLooping(true)
    citymusic:setLooping(true)
    -- importing images
    _G.emptypfp = love.graphics.newImage("images/emptyprofilephoto.png")
    _G.plant = love.graphics.newImage("images/plant.png")
    _G.smplant = love.graphics.newImage("images/smallplant.png")
    _G.book = love.graphics.newImage("images/book.png")
    _G.smbook = love.graphics.newImage("images/smbook.png")
    _G.bear = love.graphics.newImage("images/bear.png")
    _G.smbear = love.graphics.newImage("images/smbear.png")
    _G.cafe = love.graphics.newImage("images/cafeve.png")
    _G.library = love.graphics.newImage("images/libraryve.png")
    _G.forest = love.graphics.newImage("images/forestve.png")
    _G.window = love.graphics.newImage("images/windowve.png")
    _G.shelf = love.graphics.newImage("images/shelf.png")
end

-- sign up system (creating input boxes, buttons and creating account on db)
local function signUp ()
    suit.layout:reset(160,200)
    love.graphics.setFont(font.type.fontmain)
    love.graphics.setColor(1,1,1)
    -- user entering their username
    if app.state.userName == true  and app.state.signup == true then
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,50)
        suit.layout:reset(50,260)
        -- validation for entering username
        if suit.Button("Next", {id = 1}, suit.layout:row(310,30)).hit then
            -- parameters
            if input.text:len() <= 20 and input.text:len() >= 1 and input.text ~= "" then
                _G.userName = input.text
                -- opening database to insert username
                sqlite3.open("database/userDatabase.db")
                sqlite3.execute("INSERT INTO userData (userName) VALUES"..userName)
                sqlite3.close()
                input.text = ""
                acceptedUsername = true
                app.state.userEmail = true
                app.state.userName = false
            else
                acceptedUsername = false
            end
        end
        suit.layout:row()
    end
    -- user entering their email
    if app.state.userEmail == true then
        suit.layout:reset(120, 250)
        suit.Input(input, suit.layout:row(240,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,310)
        -- validation for entering email
        if suit.Button("Next", {id = 2}, suit.layout:row(310,30)).hit then
            -- parameters
            if input.text:find('@') ~= nil and input.text:find('%.') ~= nil then
                _G.userEmail = input.text
                -- opening database to insert email
                sqlite3.open("database/userDatabase.db")
                sqlite3.execute("INSERT INTO userData (userEmail) VALUES"..userEmail.." WHERE userName ="..userName)
                sqlite3.close()
                input.text = ""
                acceptedEmail = true
                app.state.userPassword = true
                app.state.userEmail = false
            else
                acceptedEmail = false
            end
        end
        suit.layout:row()
    end
    -- user entering their password
    if app.state.userPassword == true  and app.state.signup == true then
        suit.layout:reset(160,300)
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,380)
        -- validation for entering password
        if suit.Button("Create Account", {id = 3}, suit.layout:row(310,30)).hit then
            -- parameters
            if input.text:len() >= 8 and input.text ~= "" and input.text:find('[A-Z]') ~= nil and 
            input.text:find('[0-9]') ~= nil and input.text:find('[%p]') ~= nil then
                _G.userPassword = input.text
                -- hashing algorithm
                sha1.hex(userPassword)
                -- opening database to insert password
                sqlite3.open("database/userDatabase.db")
                sqlite3.execute("INSERT INTO userData (userPassword) VALUES"..userPassword.." WHERE userName ="..userName)
                sqlite3.close()
                input.text = ""
                acceptedPassword = true
                app.state.menu = true
                app.state.signup = false
                app.state.userPassword = false
            else
                acceptedPassword = false
            end
        end
        suit.layout:row()
    end
    suit.layout:reset(100,700)
    if suit.Button("Close", suit.layout:row(200,30)).hit then
        love.event.quit()
    end
    suit.layout:reset(100,550)
    -- button to change to the log in area
    if suit.Button("Log In", {id = 4},suit.layout:row(200,30)).hit then
        app.state.signup = false
        app.state.login = true
        acceptedUsername = true
        acceptedEmail = true
        acceptedPassword = true
        app.state.userName = true
        app.state.userEmail = false
        app.state.userPassword = false
    end
end

-- log in system (creating input boxes, buttons and interacting with db)
local function logIn ()
    suit.layout:reset(160,200)
    love.graphics.setFont(font.type.fontmain)
    love.graphics.setColor(1,1,1)
    -- user entering username
    if app.state.userName == true  and app.state.login == true then
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,50)
        suit.layout:reset(50,250)
        if suit.Button("Next", {id = 5}, suit.layout:row(310,30)).hit then
            _G.userName = input.text
            input.text = ""
            app.state.userPassword = true
            app.state.userName = false
        end
        suit.layout:row()
    end
    -- user entering password
    if app.state.userPassword == true  and app.state.login == true then
        suit.layout:reset(160,250)
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,310)
        if suit.Button("Log In", {id = 6}, suit.layout:row(310,30)).hit then
            _G.userPassword = input.text
            -- opening database to insert password
            sqlite3.open("database/userDatabase.db")
            if sqlite3.execute("SELECT userPassword FROM userData WHERE userName = "..userName) 
            == sha1.hex(userPassword) then
                input.text = ""
                acceptedPassword = true
                app.state.menu = true
                app.state.login = false
                app.state.userPassword = false
                squlite3.close()
            else
                acceptedPassword = false
            end
        end
        suit.layout:row()
    end
    suit.layout:reset(100,700)
    if suit.Button("Close", suit.layout:row(200,30)).hit then
        love.event.quit()
    end
    suit.layout:reset(100,500)
    -- button to go to the signup area
    if suit.Button("Sign Up", {id = 7}, suit.layout:row(200,30)).hit then
        app.state.signup = true
        app.state.login = false
        acceptedUsername = true
        acceptedEmail = true
        acceptedPassword = true
        app.state.userName = true
        app.state.userPassword = false
    end
end

-- menu area, popout menu and where to start focusing
local function menu()
    love.graphics.setFont(font.type.fontmain)
    -- button to open the pop out menu
    if app.state.popOut == false then
        suit.layout:reset(350,16)
        if suit.Button("<", suit.layout:row(30,20)).hit then
            app.state.popOut = true
        end
    end
    -- buttons to change the selected object
    suit.layout:reset(320, 400)
    if suit.Button("-->", suit.layout:row(50,20)).hit then
        if item <= 3 then
            item = item + 1
        end
        if item > 3 then
            item = 1
        end
    end
    suit.layout:reset(30, 400)
    if suit.Button("<--", suit.layout:row(50,20)).hit then
        if item >= 1 then
            item = item - 1
        end
        if item < 1 then
            item = 3
        end
    end
    love.graphics.setFont(font.type.fontlarge)
    suit.layout:reset(150, 150)
    suit.Input(input, suit.layout:row(100,50))
    suit.layout:reset(100,550)
    love.graphics.setFont(font.type.fontmain)
    -- validating the timer inputted
    if suit.Button("start", suit.layout:row(200,30)).hit then
        if input.text ~= "" and input.text:find('[A-Z]') == nil and input.text:find('[a-z]') == nil and input.text:find('[0-9]') ~= nil and input.text:find('[%p]') == nil then
            tonumber(input.text)
            seconds = input.text
            timeAdded = seconds
            app.state.ve = true
            app.state.menu = false
            acceptedTime = true
        else
            acceptedTime = false
        end
    end
    -- buttons displayed when the pop out menu is open
    if app.state.popOut == true then
        suit.layout:reset(280,60)
        love.graphics.setFont(font.type.fontmain)
        if suit.Button("Account", suit.layout:row(100,30)).hit then
            app.state.account = true
            app.state.menu = false
            app.state.popOut = false
        end
        suit.layout:row(75,10)
        if suit.Button("Settings", suit.layout:row(100,30)).hit then
            app.state.settings = true
            app.state.menu = false
            app.state.popOut = false
        end
        suit.layout:row(75,10)
        if suit.Button("Display", suit.layout:row(100,30)).hit then
            app.state.display = true
            app.state.menu = false
            app.state.popOut = false
        end
        suit.layout:row(75,10)
        if suit.Button("Close", suit.layout:row(100,30)).hit then
            love.event.quit()
        end
        -- button to close pop out menu
        suit.layout:reset(250,16)
        if suit.Button(">", suit.layout:row(30,20)).hit then
            app.state.popOut = false
        end
    end
end

-- settings, able to change the volume, the start of the week, 
-- language and sound effects
local function settings()
    suit.Slider(slider, 150, 200, 200, 30)
    suit.layout:reset(60, 280)
    -- adjustments
    if suit.Button("English", suit.layout:row(100,30)).hit then
        selectLanguage = "English"
    end
    suit.layout:reset(60, 350)
    if suit.Button("On", {id = 8}, suit.layout:row(100,30)).hit then
        musicSelected = true
    end
    suit.layout:reset(175, 350)
    if suit.Button("Off", {id = 9},suit.layout:row(100,30)).hit then
        musicSelected = false
    end
    suit.layout:reset(60, 415)
    if suit.Button("On", {id = 10}, suit.layout:row(100,30)).hit then
        soundefSelected = true
    end
    suit.layout:reset(175, 415)
    if suit.Button("Off", {id = 11},suit.layout:row(100,30)).hit then
        soundefSelected = false
    end
    suit.layout:reset(100,700)
    if suit.Button("Return to Menu", suit.layout:row(200,30)).hit then
        app.state.settings = false
        app.state.menu = true
    end
end

-- account area, displays users information and focus time
-- user can change password, email and profile photo
local function account()
    suit.layout:reset(100,300)
    -- adjustments
    if suit.Button("Change Password", suit.layout:row(200,30)).hit then
        app.state.chpassword = true
        app.state.currentp = true
        app.state.account = false
    end
    suit.layout:reset(100,350)
    if suit.Button("Change Email", suit.layout:row(200,30)).hit then
        app.state.chemail = true
        app.state.userPassword = true
        app.state.account = false
    end
    suit.layout:reset(100,400)
    if suit.Button("Change Photo", suit.layout:row(200,30)).hit then
        app.state.chpfp = true
        app.state.account = false
    end
    suit.layout:reset(100,450)
    if suit.Button("Log Out", suit.layout:row(200,30)).hit then
        app.state.account = false
        app.state.login = true
        app.state.userName = true
    end
    suit.layout:reset(100,700)
    if suit.Button("Return to Menu", suit.layout:row(200,30)).hit then
        app.state.account = false
        app.state.menu = true
    end
end

-- resetting password, inputting the previous password then
-- resetting with validation
local function resetPassword()
    -- user inputting current password
    if app.state.currentp == true then
        suit.layout:reset(160,200)
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,275)
        -- validation for entering password
        if suit.Button("Next", suit.layout:row(310,30)).hit then
            _G.userPassword = input.text
            -- opening database to check password
            sqlite3.open("database/userDatabase.db")
            if sqlite3.execute("SELECT userPassword FROM userData WHERE userName = "..userName) 
            == sha1.hex(userPassword) then
                input.text = ""
                acceptedPassword = true
                app.state.currentp = false
                app.state.newp = true
                sqlite3.close()
            else
                acceptedPassword = false
            end
        end
    end
    -- user inputting new password
    if app.state.newp == true then
        suit.layout:reset(160,255)
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,350)
        -- validation for new password
        if suit.Button("Reset Password", suit.layout:row(310,30)).hit then
            if input.text:len() >= 8 and input.text ~= "" and input.text:find('[A-Z]') ~= nil 
            and input.text:find('[0-9]') ~= nil and input.text:find('[%p]') ~= nil then
                _G.userPassword = input.text
                -- hashing algorithm
                sha1.hex(userPassword)
                -- opening database to insert new password
                sqlite3.open("database/userDatabase.db")
                sqlite3.execute("INSERT INTO userData (userPassword) VALUES "..userPassword)
                input.text = ""
                acceptedPassword = true
                app.state.account = true
                app.state.chpassword = false
                app.state.newp = false
                sqlite3.close()
            else
                acceptedPassword = false
            end
        end
    end
    suit.layout:reset(100,700)
    if suit.Button("Return to Account", suit.layout:row(200,30)).hit then
        app.state.chpassword = false
        app.state.currentp = false
        app.state.newp = false
        app.state.account = true
    end
end

-- resetting email, inputting the password, previous email
-- then the new email being reset with validation
local function resetEmail()
    -- user inputting current password
    if app.state.userPassword == true then
        suit.layout:reset(160,200)
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,260)
        -- validation for entering password
        if suit.Button("Next", {id = 12}, suit.layout:row(310,30)).hit then
            _G.userPassword = input.text
            -- opening database to check password
            sqlite3.open("database/userDatabase.db")
            if sqlite3.execute("SELECT userPassword FROM userData WHERE userName = "..userName) 
            == sha1.hex(userPassword) then
                input.text = ""
                acceptedPassword = true
                app.state.currente = true
                app.state.userPassword = false
                sqlite3.close()
            else
                acceptedPassword = false
            end
        end
    end
    -- user inputting current email
    if app.state.currente == true then
        suit.layout:reset(160,255)
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,320)
        -- validation for entering email
        if suit.Button("Next", {id = 13}, suit.layout:row(310,30)).hit then
            _G.userEmail = input.text
            -- opening database to check email
            sqlite3.open("database/userDatabase.db")
            if sqlite3.execute("SELECT userEmail FROM userData WHERE userName ="..userName) 
            == userEmail then
                input.text = ""
                acceptedEmail = true
                app.state.currente = false
                app.state.newe = true
                sqlite3.close()
            else
                acceptedEmail = false
            end
        end
    end
    -- user inputting new email
    if app.state.newe == true then
        suit.layout:reset(160,320)
        suit.Input(input, suit.layout:row(200,30))
        suit.layout:row(100,60)
        suit.layout:reset(50,395)
        -- validation for entering email
        if suit.Button("Reset Email", suit.layout:row(310,30)).hit then
            if input.text:find('@') ~= nil and input.text:find('%.') ~= nil then
                _G.userEmail = input.text
                -- opening database to insert new email
                sqlite3.open("database/userDatabase.db")
                sqlite3.execute("INSERT INTO userData (userEmail) VALUES "..userEmail)
                sqlite3.close()
                input.text = ""
                acceptedEmail = true
                app.state.account = true
                app.state.chemail = false
                app.state.newe = false
            else
                acceptedEmail = false
            end
        end
    end
    suit.layout:reset(100,700)
    if suit.Button("Return to Account", suit.layout:row(200,30)).hit then
        app.state.chemail = false
        app.state.currente = false
        app.state.newe = false
        app.state.account = true
    end
end

-- the virtual environment function, allows the user to change
-- environment and includes a pop out menu
local function ve()
    suit.layout:reset(340,770)
    love.graphics.setFont(font.type.font2main)
    -- buttons for changing environment
    if suit.Button("-->", suit.layout:row(50,20)).hit then
        if veselect <= 4 then
            veselect = veselect + 1
        end
        if veselect > 4 then
            veselect = 1
        end
    end
    suit.layout:reset(10,770)
    if suit.Button("<--", suit.layout:row(50,20)).hit then
        if veselect >= 1 then
            veselect = veselect - 1
        end
        if veselect < 1 then
            veselect = 4
        end
    end
    -- button to open pop out menu
    love.graphics.setFont(font.type.fontmain)
    if app.state.popOut == false then
        suit.layout:reset(350,16)
        if suit.Button("<", suit.layout:row(30,20)).hit then
            app.state.popOut = true
        end
    end
    -- buttons displayed when pop out menu is open
    if app.state.popOut == true then
        suit.layout:reset(280,60)
        if suit.Button("Account", suit.layout:row(100,30)).hit then
            app.state.account = true
            app.state.ve = false
        end
        suit.layout:row(75,10)
        if suit.Button("Settings", suit.layout:row(100,30)).hit then
            app.state.settings = true
            app.state.ve = false
        end
        suit.layout:row(75,10)
        if suit.Button("Display", suit.layout:row(100,30)).hit then
            app.state.display = true
            app.state.ve = false
            app.state.popOut = false
        end
        suit.layout:row(75,10)
        if suit.Button("Menu", suit.layout:row(100,30)).hit then
            app.state.menu = true
            app.state.ve = false
            app.state.popOut = false
        end
        suit.layout:row(75,10)
        if suit.Button("Close", suit.layout:row(100,30)).hit then
            love.event.quit()
        end
        -- button to close pop out menu
        suit.layout:reset(250,16)
        if suit.Button(">", suit.layout:row(30,20)).hit then
            app.state.popOut = false
        end
    end
end

-- the display area function, allows the user to place their collected items
local function display()
    suit.layout:reset(20,90)
    love.graphics.setFont(font.type.fontmain)
    -- button to open the pop out menu
    if app.state.popOut == false then
        if suit.Button("v", suit.layout:row(40, 20)).hit then
            app.state.popOut = true
        end
    end
    suit.layout:reset(180,85)
    if suit.Button("Return to Menu", suit.layout:row(200, 25)).hit then
        app.state.display = false
        app.state.menu = true
        app.state.popOut = false
    end
    -- buttons displayed when the pop out menu is open
    if app.state.popOut == true then
        -- button to close the pop out menu
        suit.layout:reset(20,90)
        if suit.Button("^", suit.layout:row(40, 20)).hit then
            app.state.popOut = false
        end
        -- displays the items to be displayed by the user
        love.graphics.setFont(font.type.fontsmall)
        if storage.spaces.slot1 ~= "" then
            suit.layout:reset(50,270)
            if suit.Button("Place Item", {id = 14}, suit.layout:row(100,20)).hit then
                spot1place = true
            end
        end
        if storage.spaces.slot2 ~= "" then
            suit.layout:reset(50,445)
            if suit.Button("Place Item", {id = 15}, suit.layout:row(100,20)).hit then
                spot2place = true
            end
        end
        if storage.spaces.slot3 ~= "" then
            suit.layout:reset(50,620)
            if suit.Button("Place Item", {id = 16}, suit.layout:row(100,20)).hit then
                spot3place = true
            end
        end
    end
end

-- runs all functions in certain areas of the app
function love.update(dt)
    if app.state.signup == true then
	    signUp()
    end
    if app.state.login == true then
        logIn()
    end
    if app.state.menu == true then
        menu()
    end
    if app.state.settings == true then
        settings()
    end
    if app.state.account == true then
        account()
    end
    if app.state.chpassword == true then
        resetPassword()
    end
    if app.state.chemail == true then
        resetEmail()
    end
    if app.state.ve == true then
        ve()
        timer:update(dt)
    end
    if app.state.display == true then
        display()
    end
end

-- all drawing aspects of the app (text, shapes etc.)
function love.draw()
    -- signup area
    if app.state.signup == true then
        -- creates border
        love.graphics.setColor(1,1,1)
        love.graphics.rectangle("fill", 20, 20, 360, 760)
        suit.draw()
        love.graphics.setFont(font.type.fontlarge)
        -- creates box for login button
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        love.graphics.rectangle("line", 80, 450, 240, 160)
        -- title
        love.graphics.print("Welcome to Halcyon!", 55, 60)
        love.graphics.setFont(font.type.font2large)
        love.graphics.print("Sign Up", 135, 120)
        love.graphics.print("Already have\n an account?", 95, 470)
        love.graphics.setFont(font.type.fontmain)
        -- user entering username
        if app.state.userName == true then
            love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
            love.graphics.print("username:", 50, 200)
            if acceptedUsername == false then
                love.graphics.setFont(font.type.fontsmall)
                love.graphics.setColor(1,0,0)
                -- error message
                love.graphics.print("username must contain between 0 and 20 characters", 60, 235)
            end
        end
        -- user entering email
        if app.state.userEmail == true then
            love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
            love.graphics.print("email:", 50, 255)
            if acceptedEmail == false then
                love.graphics.setFont(font.type.fontsmall)
                love.graphics.setColor(1,0,0)
                -- error message
                love.graphics.print("email must contain '@' and at least one '.'", 80, 285)
            end
        end
        -- user entering password
        if app.state.userPassword == true then
            love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
            love.graphics.print("password:", 50, 305)
            if acceptedPassword == false then
                love.graphics.setFont(font.type.fontsmall)
                love.graphics.setColor(1,0,0)
                -- error message
                love.graphics.print("password must be at least 8 characters and contain\na capital letter, number and special character", 60, 335)
            end
        end
    end
    -- login area
    if app.state.login == true then
        -- creates border
        love.graphics.setColor(1,1,1)
        love.graphics.rectangle("fill", 20, 20, 360, 760)
        suit.draw()
        love.graphics.setFont(font.type.fontlarge)
        -- creates box for signup button
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        love.graphics.rectangle("line", 80, 400, 240, 160)
        -- title
        love.graphics.print("Welcome to Halcyon!", 55, 60)
        love.graphics.setFont(font.type.font2large)
        love.graphics.print("Log In", 140, 120)
        love.graphics.print("Don't have\n an account?", 100, 420)
        love.graphics.setFont(font.type.fontmain)
        -- user entering username
        if app.state.userName == true then
            love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
            love.graphics.print("username:", 50, 200)
        end
        -- user entering passwor
        if app.state.userPassword == true then
            love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
            love.graphics.print("password:", 50, 250)
            if acceptedPassword == false then
                love.graphics.setFont(font.type.fontsmall)
                love.graphics.setColor(1,0,0)
                -- error message
                love.graphics.print("username or password is incorrect", 110, 285)
            end
        end
    end
    -- menu area
    if app.state.menu == true then
        -- stops audio from ve
        love.audio.stop()
        -- creates bar at top of the screen
        love.graphics.setColor(1,1,1)
        love.graphics.rectangle("fill", 0, 0, 400, 50)
        suit.draw()
        love.graphics.setFont(font.type.fontmain)
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        -- title
        love.graphics.print("Welcome back, ".. userName .."!",10, 15)
        -- appears when pop out menu open
        if app.state.popOut == true then
            love.graphics.print("Home", 290, 15)
        end
        love.graphics.setColor(1,1,1)
        love.graphics.print("time to focus!", 135, 100)
        -- creates circle to hold item
        love.graphics.circle("fill", 200, 350, 120)
        -- which object is displayed
        if item == 1 then
            -- plant
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(plant, 105, 258)
        end
        if item == 2 then
            -- book
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(book, 103, 258)
        end
        if item == 3 then
            -- bear
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(bear, 113, 253)
        end
        if acceptedTime == false then
            love.graphics.setColor(1,1,1)
            love.graphics.setFont(font.type.fontsmall)
            -- error
            love.graphics.print("Entered time must not contain any\n    letters or special characters", 105, 495)
        end
    end
    -- settings area
    if app.state.settings == true then
        -- stops audio from ve
        love.audio.stop()
        -- creates border
        love.graphics.setColor(1,1,1)
        love.graphics.rectangle("fill", 20, 20, 360, 760)
        suit.draw()
        love.graphics.setFont(font.type.font2large)
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        -- title
        love.graphics.print("Settings", 130, 60)
        love.graphics.setFont(font.type.fontmain)
        -- headings for adjustments
        love.graphics.print("volume:", 60, 200)
        love.graphics.print("language: "..selectLanguage, 60, 245)
        love.graphics.print("music:", 60, 320)
        love.graphics.print("sound effects:", 60, 385)
        -- adjust music
        if musicSelected == true then
            love.graphics.print("On", 150, 320)
        else
            love.graphics.print("Off", 150, 320)
        end
        -- adjust sound effect
        if soundefSelected == true then
            love.graphics.print("On", 220, 385)
        else
            love.graphics.print("Off", 220, 385)
        end
    end
    -- account area
    if app.state.account == true then
        -- creates border
        love.graphics.setColor(1,1,1)
        love.graphics.rectangle("fill", 20, 20, 360, 760)
        suit.draw()
        love.graphics.setFont(font.type.font2large)
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        -- title
        love.graphics.print("My Account", 110, 60)
        love.graphics.setFont(font.type.font3small)
        -- opening database to display values from it
        --sqlite3.open("database/userDatabase.db")
        love.graphics.print("username: "..userName, 180, 140)
        -- love.graphics.print("email: "..sqlite3.execute("SELECT userEmail FROM userData WHERE userName ="..userName), 180, 160)
        love.graphics.print("time focused: "..totalTime.." secs", 180, 180)
        love.graphics.print("items collected: "..totalItems, 180, 200)
        --sqlite3.close()
        love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
        love.graphics.draw(emptypfp, 40, 110)
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        love.graphics.setFont(font.type.fontmain)
    end
    -- changing password
    if app.state.chpassword == true then
        -- creating border
        love.graphics.setColor(1,1,1)
        love.graphics.rectangle("fill", 20, 20, 360, 760)
        suit.draw()
        love.graphics.setFont(font.type.font2large)
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        -- title
        love.graphics.print("Reset Password", 70, 60)
        -- user entering current password
        if app.state.currentp == true then
            love.graphics.setFont(font.type.fontmain)
            love.graphics.print("  current\npassword:", 50, 200)
            if acceptedPassword == false then
                love.graphics.setColor(1,0,0)
                love.graphics.setFont(font.type.fontsmall)
                -- error message
                love.graphics.print("password is not correct" , 180, 235)
                love.graphics.setFont(font.type.fontmain)
            end
        end
        -- user entering new password
        if app.state.newp == true then
            love.graphics.setFont(font.type.fontmain)
            -- error message
            love.graphics.print("     new\npassword:", 50, 255)
            if acceptedPassword == false then
                love.graphics.setColor(1,0,0)
                love.graphics.setFont(font.type.fontsmall)
                -- error message
                love.graphics.print("password must be at least 8 characters and contain\na capital letter, number and special character", 60, 308)
                love.graphics.setFont(font.type.fontmain)
            end
        end
    end
    -- changing email
    if app.state.chemail == true then
        -- creatin border
        love.graphics.setColor(1,1,1)
        love.graphics.rectangle("fill", 20, 20, 360, 760)
        suit.draw()
        love.graphics.setFont(font.type.font2large)
        love.graphics.setColor{0.3921568627451, 0.5843137254902, 0.92941176470588}
        -- title
        love.graphics.print("Reset Email", 100, 60)
        -- user entering current password
        if app.state.userPassword == true then
            love.graphics.setFont(font.type.fontmain)
            love.graphics.print("password:", 50, 205)
            if acceptedPassword == false then
                love.graphics.setColor(1,0,0)
                love.graphics.setFont(font.type.fontsmall)
                -- error message
                love.graphics.print("password does not match" , 190, 235)
                love.graphics.setFont(font.type.fontmain)
            end
        end
        -- user entering current email
        if app.state.currente == true then
            love.graphics.setFont(font.type.fontmain)
            love.graphics.print("current\nemail:", 50, 255)
            if acceptedEmail == false then
                love.graphics.setColor(1,0,0)
                love.graphics.setFont(font.type.fontsmall)
                -- error
                love.graphics.print("email does not match" , 190, 290)
                love.graphics.setFont(font.type.fontmain)
            end
        end
        -- user entering new email
        if app.state.newe == true then
            love.graphics.setFont(font.type.fontmain)
            love.graphics.print("new\nemail:", 50, 320)
            if acceptedEmail == false then
                love.graphics.setColor(1,0,0)
                love.graphics.setFont(font.type.fontsmall)
                -- error message
                love.graphics.print("email must contain '@' and\n at least one '.'", 170, 355)
                love.graphics.setFont(font.type.fontmain)
            end
        end
    end
    -- virtual environment
    if app.state.ve == true then
        -- changing environment using integer
        if veselect == 1 then
            librarymusic:pause()
            citymusic:pause()
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(cafe, -100, -50)
            cafemusic:play()
            suit.draw()
        end
        if veselect == 2 then
            cafemusic:pause()
            forestmusic:pause()
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(library, -100, -50)
            librarymusic:play()
            suit.draw()
        end
        if veselect == 3 then
            librarymusic:pause()
            citymusic:pause()
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(forest, -200, -100)
            forestmusic:play()
            suit.draw()
        end
        if veselect == 4 then
            forestmusic:pause()
            cafemusic:pause()
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(window, -150, -100)
            citymusic:play()
            suit.draw()
        end
        -- creating circle to hold item
        love.graphics.setColor(1,1,1)
        love.graphics.circle("fill", 75, 75, 65)
        -- which item is selected
        if item == 1 then
            itemName = "plant"
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(smplant, 28, 26)
        end
        if item == 2 then
            itemName = "book"
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(smbook, 26, 26)
        end
        if item == 3 then
            itemName = "bear"
            love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
            love.graphics.draw(smbear, 33, 26)
        end
        love.graphics.setColor(1,1,1)
        -- displaying timer on-screen
        love.graphics.setFont(font.type.timerfont)
        if tonumber(seconds) >= 0 then
            love.graphics.print(seconds, 150, 40)
        else
            -- where to store item
            if storage.spaces.slot1 == "" then
                storage.spaces.slot1 = itemName
            elseif storage.spaces.slot2 == "" then
                storage.spaces.slot2 = itemName
            else
                storage.spaces.slot3 = itemName
            end
            -- opening database to update values
            sqlite3.open("database/userDatabase.db")
            totalTime = timeAdded + sqlite3.execute("SELECT timeTotal FROM userData WHERE userName = ".. userName)
            sqlite3.execute("INSERT INTO userData (timeTotal) VALUES "..totalTime.." WHERE userName = ".. userName)
            totalItems = totalItems + sqlite3.execute("SELECT itemTotal FROM userData WHERE userName = " .. userName)
            sqlite3.execute("INSERT INTO userData (itemTotal) VALUES "..totalItems.." WHERE userName = " .. userName)
            sqlite3.close()
            app.state.ve = false
            app.state.display = true
        end
        -- displayed when pop out menu open
        if app.state.popOut == true then
            love.graphics.setFont(font.type.fontmain)
            love.graphics.setColor(1,1,1)
            love.graphics.print("Home", 290, 15)
        end
    end
    -- display area
    if app.state.display == true then
        -- stopping music from ve
        love.audio.stop()
        love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
        love.graphics.draw(shelf, 0, 120)
        love.graphics.setFont(font.type.font2large)
        love.graphics.setColor{1,1,1}
        -- title
        love.graphics.print(userName.."'s Display", 22, 40)
        suit.draw()
        -- which item is displayed where in pop out menu
        if app.state.popOut == true then
            if storage.spaces.slot1 ~= "" then
                love.graphics.setColor(1,1,1)
                love.graphics.circle("fill", 100, 200, 65)
                if storage.spaces.slot1 == "plant" then
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smplant, 55, 152)
                elseif storage.spaces.slot1 == "book" then
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smbook, 52, 150)
                else
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smbear, 60, 150)
                end
            end
            if storage.spaces.slot2 ~= "" then
                love.graphics.setColor(1,1,1)
                love.graphics.circle("fill", 100, 370, 65)
                if storage.spaces.slot2 == "plant" then
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smplant, 55, 322)
                elseif storage.spaces.slot2 == "book" then
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smbook, 52, 322)
                else
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smbear, 60, 322)
                end
            end
            if storage.spaces.slot3 ~= "" then
                love.graphics.setColor(1,1,1)
                love.graphics.circle("fill", 100, 545, 65)
                if storage.spaces.slot3 == "plant" then
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smplant, 55, 492)
                elseif storage.spaces.slot3 == "book" then
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smbook, 52, 495)
                else
                    love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                    love.graphics.draw(smbear, 60, 487)
                end
            end
        end
        -- updating spots when an item is placed
        if spot1place == true then
            storage.places.place1 = storage.spaces.slot1
            storage.spaces.slot1 = storage.spaces.slot2
            storage.spaces.slot2 = storage.spaces.slot3
            storage.spaces.slot3 = ""
            spot1place = false
        end
        if spot2place == true then
            storage.places.place2 = storage.spaces.slot2
            storage.spaces.slot2 = storage.spaces.slot3
            storage.spaces.slot3 = ""
            spot2place = false
        end
        if spot3place == true then
            storage.places.place3 = storage.spaces.slot3
            storage.spaces.slot3 = ""
            spot3place = false
        end
        -- where item is displayed on shelf
        if storage.places.place1 ~= "" then
            if storage.places.place1 == "plant" then
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smplant, 275, 217)
            elseif storage.places.place1 == "book" then
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smbook, 260, 232)
            else
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smbear, 275, 235)
            end
        end
        if storage.places.place2 ~= "" then
            if storage.places.place2 == "plant" then
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smplant, 150, 520)
            elseif storage.places.place2 == "book" then
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smbook, 150, 535)
            else
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smbear, 150, 530)
            end
        end
        if storage.places.place3 ~= "" then
            if storage.places.place3 == "plant" then
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smplant, 50, 325)
            elseif storage.places.place3 == "book" then
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smbook, 50, 330)
            else
                love.graphics.setColor{0.82745098039216, 0.82745098039216, 0.82745098039216}
                love.graphics.draw(smbear, 50, 335)
            end
        end
        love.graphics.setColor(1,1,1)
        love.graphics.setFont(font.type.fontmain)
        love.graphics.print("total time focused: "..totalTime.." secs", 10, 700)
        love.graphics.print("items collected: "..totalItems, 10, 740)
    end
end

function love.textedited(text, start, length)
    suit.textedited(text, start, length)
end

function love.textinput(t)
    suit.textinput(t)
end

function love.keypressed(key)
    suit.keypressed(key)
end
