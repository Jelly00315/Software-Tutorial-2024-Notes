# Tutorial 2: IOs on Embed System 
[Back to Main](README.md)
<details>
<summary>Authors</summary>

Ivan Lok(yfilok@connect.ust.hk)
Modified from:
Dicaprio Cheung
Joseph Lam, Binay Gurung, Anshuman Medhi
</details>


> A important note for all you guys before everything start:
> Please **DO NOT** put the mainboard on your computer or any metal surface.
> It can burn both your computer and also the pcb. 

## Basic Structure in the `main.c`

After we have discussed some C programming technique, we are moving forward to more specific programming under STM32.

The `main.c` file for STM32 projects typically follows a structured format designed to facilitate embedded system programming. This structure is generated by STM32CubeMX and includes several key sections:

```c
int main(void) {

    /* USER CODE BEGIN 1 */
    /* USER CODE END 1 */
    
    /* MCU Configuration--------------------------------------------------------*/
    /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
    HAL_Init();
    /* USER CODE BEGIN Init */

    /* USER CODE END Init */

    /* Configure the system clock */
    SystemClock_Config();

    /* USER CODE BEGIN SysInit */

    /* USER CODE END SysInit */

    /* Initialize all configured peripherals */
    MX_GPIO_Init();
    
    //The System will call the _Init Function of all peripherals here
    
    /* USER CODE BEGIN 2 */
    /* USER CODE END 2 */

    /* Infinite loop */
    /* USER CODE BEGIN WHILE */
    while (1) {
        /* USER CODE END WHILE */
        /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
}
```

- The most notable different is that CubeMX have pre-written a `while` loop for you, and the reason is that:
    
    - Unlike normal program we written under course / competition context that our program only run once to solve a specific problem, You want your program run continuously control the robot and never loss control of the robot (program end)

    - Another important idea in mind is that you should somehow keep your main `while` loop iterate, as there will be a lot of periperal function which need to be keep calling to pass value and control periperal in your real code, below I attached a modified real code for your reference
        <details><summary>Modified Real Code</summary>

        ```c
        while (1)
        {
            CurrentTime = HAL_GetTick();
            SW1State = !gpio_read(SW1);
            SW2State = !gpio_read(SW2);
            ADC_Reading() //Function to read IR Sensor Value via ADC
            encoder_operation();
            if (!startstate && SW1State)
                startstate = 1;
            callibrationLoop(&lf);
            controlLoop(&lf);
            if (startstate){
                    Speed_Control(lf.left_speed, lf.right_speed);
            }
        }
        ```
        </details>
    - And So with this in mind, you should be always careful about some loop without a bounded number of interations

    If you have play arduino before, the `while` loop act like the `loop()`
    
- Another Different the placeholder for user self-defined code. These zone created for allowing you to insert custom functionality without interfering with auto-generated code.

    STM32CubeMX generates code with predefined sections for user modifications. This ensures that user code is preserved across code regenerations, which is required when there is any change in chip configurations.
    
    Therefore, you should always put you code inside those designated zone:
    
    ```c
        /* USER CODE BEGIN Init */
        // Your Code should write here
        /* USER CODE END Init */
        // Your Code should NOT write here
    ```
[Continue to The Next Page](./02-GPIO.md)