# simulation-of-Advanced-LED-controller
#include <stdio.h>
#include <stdint.h>


typedef struct {
    uint8_t state;        // 0 = OFF, 1 = ON
    uint8_t brightness;   // 0-255 brightness level
    uint32_t color;       // RGB color encoded as a single uint32_t value (0xRRGGBB)
} LEDSettings;

typedef struct {
    LEDSettings singleLED;   // Single LED settings
    uint8_t groupState;      // 0 = OFF, 1 = ON for the entire group
    uint8_t groupBrightness; // Brightness adjustment for the group
} LEDGroup;


void initLEDGroup(LEDGroup *group) {
    // Initialize the single LED settings to default values
    group->singleLED.state = 0;              // OFF
    group->singleLED.brightness = 0;         // Minimum brightness
    group->singleLED.color = 0x000000;       // No color (Black)
    
    // Initialize the group settings
    group->groupState = 0;                   // All OFF
    group->groupBrightness = 0;              // Minimum brightness
}


void updateLEDGroupSettings(LEDGroup *group, 
                            uint8_t groupState, 
                            uint8_t groupBrightness, 
                            uint8_t state, 
                            uint8_t brightness, 
                            uint32_t color) {
    
    group->groupState = groupState;
    group->groupBrightness = groupBrightness;

   
    group->singleLED.state = state;
    group->singleLED.brightness = brightness;
    group->singleLED.color = color;
}


void displayLEDGroupStatus(const LEDGroup *group) 
{
    // Display individual LED settings
    printf("Individual LED Settings:\n");
    printf("State: %s\n", group->singleLED.state ? "ON" : "OFF");
    printf("Brightness: %d\n", group->singleLED.brightness);
    printf("Color (RGB): #%06X\n", group->singleLED.color);

    // Display group settings
    printf("\nGroup Settings:\n");
    printf("Group State: %s\n", group->groupState ? "ALL ON" : "ALL OFF");
    printf("Group Brightness: %d\n", group->groupBrightness);
}

int main()
{
    LEDGroup livingRoomLEDs;
    initLEDGroup(&livingRoomLEDs);

    
    printf("Default LED Group Status:\n");
    displayLEDGroupStatus(&livingRoomLEDs);
    printf("\n");

    
    updateLEDGroupSettings(&livingRoomLEDs, 
                           1,       // Group ON
                           150,     // Group brightness
                           1,       // Individual LED ON
                           200,     // Individual LED brightness
                           0xFF0000 // Color: Red
    );

    printf("Updated LED Group Status:\n");
    displayLEDGroupStatus(&livingRoomLEDs);

    return 0;
}
