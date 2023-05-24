package com.example.carrinho;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.os.Bundle;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
import androidx.appcompat.app.AppCompatActivity;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class MainActivity extends AppCompatActivity {
    private GameView gameView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        gameView = new GameView(this);
        setContentView(gameView);
    }

    public class GameView extends View {
        private int posicaoCarroX;
        private int posicaoCarroY;

        private Paint paintCarro;

        private Thread threadJogo;

        private boolean rodando;

        private int pontuacao;

        private List<Carro> carros;
        private Paint paintCarroVindo;
        private Random random;

        public GameView(Context context) {
            super(context);
            iniciar();
        }

        public GameView(Context context, AttributeSet attrs) {
            super(context, attrs);
            iniciar();
        }

        private void iniciar() {
            posicaoCarroX = (getWidth() - 100) / 2;
            posicaoCarroY = getHeight();

            paintCarro = new Paint();
            paintCarro.setColor(Color.WHITE);

            carros = new ArrayList<>();
            paintCarroVindo = new Paint();
            paintCarroVindo.setColor(Color.RED);
            random = new Random();

            threadJogo = new Thread(new Runnable() {
                @Override
                public void run() {
                    while (rodando) {
                        atualizar();
                        postInvalidate();
                        try {
                            Thread.sleep(16);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                }
            });
        }

        protected void onAttachedToWindow() {
            super.onAttachedToWindow();
            rodando = true;
            threadJogo.start();
        }

        protected void onDetachedFromWindow() {
            super.onDetachedFromWindow();
            rodando = false;
            try {
                threadJogo.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        protected void onDraw(Canvas canvas) {
            super.onDraw(canvas);

            canvas.drawColor(Color.BLACK);

            Paint paintLinha = new Paint();
            paintLinha.setColor(Color.YELLOW);

            paintLinha.setStrokeWidth(20);
            canvas.drawLine(getWidth() / 2, 0, getWidth() / 2, getHeight(), paintLinha);

            canvas.drawRect(posicaoCarroX, posicaoCarroY - 100, posicaoCarroX + 100, posicaoCarroY, paintCarro);
            for (Carro carro : carros) {
                canvas.drawRect(carro.getX(), carro.getY(), carro.getX() + 100, carro.getY() + 100, paintCarroVindo);
            }

            Paint paintTexto = new Paint();
            paintTexto.setTextSize(50);
            paintTexto.setColor(Color.WHITE);
            canvas.drawText("Pontuação: " + pontuacao, getWidth() - 450, getHeight() - 50, paintTexto);

        }


        private void atualizar() {
            posicaoCarroY = getHeight() / 2 - 50;

            for (int i = 0; i < carros.size(); i++) {
                Carro carro = carros.get(i);
                carro.setY(carro.getY() + 10);
                if (carro.getY() > getHeight()) {
                    carros.remove(carro);
                    i--;
                } else {
                    if (carro.getX() < posicaoCarroX + 100 && carro.getX() + 100 > posicaoCarroX
                            && carro.getY() < posicaoCarroY + 100 && carro.getY() + 100 > posicaoCarroY) {
                        rodando = false;
                        reiniciarJogo();
                    }
                }
            }
            if (random.nextInt(100) < 4) {
                int x = random.nextInt(getWidth() - 100);
                int y = -100;
                Carro novoCarro = new Carro(x, y);
                carros.add(novoCarro);
            }
            pontuacao++;
        }

        private void reiniciarJogo() {
            posicaoCarroX = getWidth() / 2 - 50;
            posicaoCarroY = getHeight() / 2 - 50;
            carros.clear();
            pontuacao = 0;
            rodando = true;
        }


        @Override
        public boolean onTouchEvent(MotionEvent event) {
            switch (event.getAction()) {
                case MotionEvent.ACTION_DOWN:
                case MotionEvent.ACTION_MOVE:
                    posicaoCarroX = (int) event.getX() - 50;
                    break;
                default:
                    break;
            }
            return true;
        }

        public class Carro {
            private int x;
            private int y;

            public Carro(int x, int y) {
                this.x = x;
                this.y = y;
            }

            public int getX() {
                return x;
            }

            public void setX(int x) {
                this.x = x;
            }

            public int getY() {
                return y;
            }

            public void setY(int y) {
                this.y = y;
            }
        }
    }
}
