### Example FreeRTOS: task


En este programa lo que hacemos es crear 3 tareas [xTaskCreate](https://www.freertos.org/a00125.html) en **FreeRTOS**, a cada tarea le asignamos la misma función pero distinto parámetro.


```

// Creo 3 tareas
    xTaskCreate(task,"TaskA",MEM_STACK,&(blinking_A),0,&taskA);
    xTaskCreate(task,"TaskA",MEM_STACK,&(blinking_B),0,&taskB);
    xTaskCreate(task,"TaskA",MEM_STACK,&(blinking_C),0,&taskC);

```
 El parámetro es una estructura `blinking_param` que se define de la siguiente manera.

```
typedef struct 
{
    gpio_num_t led;
    uint32_t delay;
} blinking_params;

```

La función a ejecutar por las tareas es la siguiente:


```
/*
Función Task:
    Recibe un parámetro blinking_param como puntero
    Configura el led definido en el parámetro recibido    (param->led)
    Configura el delay definido en el parámetro recibido  (param->delay)
*/
void task( void* param){
    blinking_params *p = (blinking_params*)param;
    gpio_set_direction((p)->led,GPIO_MODE_OUTPUT);


    bool level = true;

    while(1){
        if(level){
            level = false;
            gpio_set_level(p->led,1);
        }
        else{
            level = true;
            gpio_set_level(p->led,0);
        }
        vTaskDelay((p)->delay/portTICK_PERIOD_MS);
    }
}


```


El resultado en el analizador lógico es el siguiente:

![blinking_leds](./imgs/LEDS_BLINKING.png)