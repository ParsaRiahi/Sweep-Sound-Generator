# Sweep-Sound-Generator
Originally called Exercise7.c

// Exercise 7

#include <stdio.h>
#include <math.h>

#define SAMPLE_RATE 48000
#define DURATION 10
#define NUM_SAMPLES (SAMPLE_RATE * DURATION)

int main(void) {

    FILE *file = fopen("sweep.raw", "wb");

    if(!file) 
    {
        perror("Error opening file");
        return 1;
    }

    float f_start = 1.0;
    float f_end =(float)SAMPLE_RATE;
    float t;
    float sample;
    float k = log10(f_end / f_start) / DURATION;

    for(int i = 0; i < NUM_SAMPLES; i++) 
    {
        t =(float)i / SAMPLE_RATE;             
        float freq = f_start * pow(10, k * t);         
        sample = sin(2.0 * M_PI * freq * t);    
        fwrite(&sample, sizeof(float), 1, file);
    }

    fclose(file);
    printf("Sweep written to sweep.raw\n");

    return 0;
}
