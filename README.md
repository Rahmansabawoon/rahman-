#include <stdio.h>
#include <math.h>

#define NUM_MAHALLE 3

// Mahalle verileri
typedef struct {
    char name[20];
    double population_density;       // Nüfus yoğunluğu
    double transport_infrastructure; // Ulaşım altyapısı
    double cost;                     // Maliyet
    double environmental_impact;     // Çevresel etki
    double social_benefit;           // Sosyal fayda
} Mahalle;

// Ağırlıklar
double weights[] = {0.25, 0.20, -0.15, 0.20, 0.20};

// Softmax fonksiyonu
void softmax(double scores[], double probabilities[], int size) {
    double max_score = scores[0];
    for (int i = 1; i < size; i++) {
        if (scores[i] > max_score) {
            max_score = scores[i];
        }
    }

    double sum_exp_scores = 0.0;
    for (int i = 0; i < size; i++) {
        scores[i] = exp(scores[i] - max_score);
        sum_exp_scores += scores[i];
    }

    for (int i = 0; i < size; i++) {
        probabilities[i] = scores[i] / sum_exp_scores;
    }
}

int main() {
    Mahalle mahalleler[NUM_MAHALLE] = {
        {"Mahalle A", 300.0, 8.0, 500000.0, 7.0, 9.0},
        {"Mahalle B", 450.0, 6.0, 600000.0, 5.0, 8.0},
        {"Mahalle C", 200.0, 9.0, 400000.0, 6.0, 7.0}
    };

    // Maliyet değerlerini ölçeklendirme (binlere bölme)
    for (int i = 0; i < NUM_MAHALLE; i++) {
        mahalleler[i].cost /= 1000.0;
    }

    double scores[NUM_MAHALLE];
    double probabilities[NUM_MAHALLE];

    // Puan hesaplama
    for (int i = 0; i < NUM_MAHALLE; i++) {
        scores[i] = (weights[0] * mahalleler[i].population_density) +
                    (weights[1] * mahalleler[i].transport_infrastructure) +
                    (weights[2] * mahalleler[i].cost) +
                    (weights[3] * mahalleler[i].environmental_impact) +
                    (weights[4] * mahalleler[i].social_benefit);
    }

    // Softmax uygulama
    softmax(scores, probabilities, NUM_MAHALLE);

    // Sonuçları yazdırma
    printf("Mahalleler için olasılıklar:\n");
    for (int i = 0; i < NUM_MAHALLE; i++) {
        printf("%s: %.4f\n", mahalleler[i].name, probabilities[i]);
    }

    return 0;
}
