<script>
import { ref, onMounted, onUnmounted } from 'vue';
import * as THREE from 'three';
import {FontLoader} from "three/examples/jsm/loaders/FontLoader";
import {TextGeometry} from "three/examples/jsm/geometries/TextGeometry";
import axios from 'axios';

export default {
  name: 'CryptosCreep3d',
  props: {
    cripView3d: {
      type: Boolean,
      required: true
    },
  },
  setup() {
    const marquee = ref(null);
    let scene, camera, renderer, initialCryptos = [];

    const fetchExchangeRates = async () => {
      try {
        const response = await axios.get('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,litecoin,tether,avalanche-2,filecoin,bitcoin-cash,binancecoin,dogecoin,ripple,cardano,polkadot,chainlink,stellar&vs_currencies=usd');

        // Преобразуем ответ от API в массив данных о криптовалютах
        const selectedCryptos = Object.keys(response.data).map((id) => ({
          id,
          name: initialCryptos.find((c) => c.id === id)?.name || '',
          price: response.data[id].usd,
        }));

        selectedCryptos.forEach((crypto) => {
          createCryptosObject(crypto);
        });
      } catch (error) {
        console.error(error);
      }
    };

    let nextPositionX = 0; // Стартовая позиция для первого объекта

    const createCryptosObject = (crypto) => {
      const cryptosName = `${crypto.id} = ${crypto.price} usd`;
      const loader = new FontLoader();

      loader.load('https://threejs.org/examples/fonts/droid/droid_serif_regular.typeface.json', (font) => {
        const geometry = new TextGeometry(cryptosName, {
          font: font,
          size: 0.1,
          height: 0.01,
        });

        geometry.computeBoundingBox();
        const textWidth = geometry.boundingBox.max.x - geometry.boundingBox.min.x;

        const textureLoader = new THREE.TextureLoader();
        textureLoader.load('/assets/background/background04.webp', function (texture) {
          const material = new THREE.MeshBasicMaterial({ map: texture });
          const cryptosObject = new THREE.Mesh(geometry, material);

          // Установка начальной позиции объекта
          cryptosObject.position.x = nextPositionX;

          // Обновление позиции для следующего объекта
          nextPositionX += textWidth + 0.2;

          initialCryptos.push(cryptosObject);
          scene.add(cryptosObject);
        });
      });
    };

    const init = () => {
      scene = new THREE.Scene();
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      camera.position.z = 2.5;
      renderer = new THREE.WebGLRenderer({alpha: true});
      renderer.setSize(window.innerWidth, window.innerHeight);
      scene.add(camera);

      marquee.value.appendChild(renderer.domElement);

      fetchExchangeRates();
      animate();
    };

    const animate = () => {
      requestAnimationFrame(animate);

      const speed = 0.01; // Скорость движения

      // Используем `initialCryptos` для итерации
      initialCryptos.forEach((crypta, index) => {
        // Двигаем объекты влево
        crypta.position.x -= speed;

        // Вычисляем правую границу видимости для объекта
        if (crypta.geometry && crypta.geometry.boundingBox) {
          const objectRightEdge = crypta.position.x + (crypta.geometry.boundingBox.max.x - crypta.geometry.boundingBox.min.x);

          // Перемещаем объект обратно в начало, когда он полностью выходит за левую границу видимости
          if (objectRightEdge < -window.innerWidth / window.innerHeight * 2.5) {
            const lastCrypta = initialCryptos[initialCryptos.length - 1];
            const spaceBetween = 0.2; // Желаемое расстояние между объектами
            crypta.position.x = lastCrypta.position.x + lastCrypta.geometry.boundingBox.max.x - lastCrypta.geometry.boundingBox.min.x + spaceBetween;

            // Переупорядочиваем массив, чтобы сохранить последовательность
            initialCryptos.splice(index, 1);
            initialCryptos.push(crypta);
          }
        }
      });

      //   // Вращение всей сцены
      //   const sceneRotationAngle = 20; // Угол в градусах
      //   scene.rotation.x = THREE.MathUtils.degToRad(sceneRotationAngle);
      //
      //   // scene.rotation.x += 0.01;
      //
      //   renderer.render(scene, camera);

      renderer.render(scene, camera);
    };


    const onWindowResize = () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();

      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    };

    const updateCameraPosition = () => {
      if (window.innerWidth <= 768) {
        camera.position.z = 1.3;
      } else {
        camera.position.z = 1.3;
      }
    };

    window.addEventListener('resize', () => {
      onWindowResize();
      updateCameraPosition();
    });

    onMounted(() => {
      init();
      onWindowResize();
      updateCameraPosition();
    });

    onUnmounted(() => {
      renderer.dispose();
    });

    return {
      marquee,
      initialCryptos
    };
  },
};
</script>

<template>
  <div class="inner" v-show="cripView3d">
    <div class="marquee" ref="marquee"></div>
  </div>
</template>

<style lang="scss" scoped>
.inner {
  //margin-bottom: 1rem;
  overflow: hidden;
  position: relative;

  .marquee {
    overflow: hidden;
    background: none;
  }
}
</style>
