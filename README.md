-----------------------------------------------------------------------------------------
--
-- main.lua
--
-----------------------------------------------------------------------------------------


orientation = {
		default = "landscapeRight",
		supported = { "landscapeRight", }
	}

display.setStatusBar( display.HiddenStatusBar )

local options =
{
    width = 240,
    height = 240,
    numFrames = 1,
}

local background = display.newRect(0,0, display.contentWidth * 2, display.contentHeight * 2)
background:setFillColor( 255,255,255 )

local myButton = display.newRect( 160, 400, 300, 50 )

local frisbee = graphics.newImageSheet("Frisbee.png", options )

local frame1 = display.newImage( frisbee, 1, 160, 190 )

local background = display.newImage( "Sky.jpg", 160, 190)
background.alpha = 0

local Dragme = display.newImage("Dragme.png", 160, -200)

local function text()
	caughtdisc = display.newText( " You caught a disc! Tap here ", 160, 400, native.systemFont, 18 )
caughtdisc:setFillColor( 100, 0, 0 )

end

local function screenTap()
	transition.to( frame1, { time=1500, alpha=0, x=(160), y=(0), onComplete=text})

end

local function menuTap()
	transition.to( background, { time=1500, alpha=1 })
	transition.to( Dragme, { time=1000, x=(160), y=(300) })
	display.remove (caughtdisc)
	display.remove (myButton)
end

function Dragme:touch( event )
    if event.phase == "began" then
	
        self.markX = self.x 
        self.markY = self.y
	
    elseif event.phase == "moved" then
	
        local x = (event.x - event.xStart) + self.markX
        local y = (event.y - event.yStart) + self.markY
        
        self.x, self.y = x, y 
    end
    
    return true
end

Dragme:addEventListener( "touch", Dragme )

frame1:addEventListener( "tap", screenTap )

myButton:addEventListener( "tap", menuTap )
