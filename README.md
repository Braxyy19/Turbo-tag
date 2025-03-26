import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

let scene, camera, renderer, controls;
let player, chaser;
let speed = 0.1;

init();
animate();

function init() {
    scene = new THREE.Scene();
    scene.background = new THREE.Color(0x87ceeb);

    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.set(0, 5, 10);

    renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    controls = new OrbitControls(camera, renderer.domElement);
    
    const ground = new THREE.Mesh(
        new THREE.PlaneGeometry(50, 50),
        new THREE.MeshBasicMaterial({ color: 0x228B22 })
    );
    ground.rotation.x = -Math.PI / 2;
    scene.add(ground);

    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });
    player = new THREE.Mesh(geometry, material);
    player.position.set(0, 0.5, 0);
    scene.add(player);

    const chaserMaterial = new THREE.MeshBasicMaterial({ color: 0x0000ff });
    chaser = new THREE.Mesh(geometry, chaserMaterial);
    chaser.position.set(5, 0.5, 0);
    scene.add(chaser);

    window.addEventListener('keydown', movePlayer);
}

function movePlayer(event) {
    switch (event.key) {
        case 'w': player.position.z -= speed; break;
        case 's': player.position.z += speed; break;
        case 'a': player.position.x -= speed; break;
        case 'd': player.position.x += speed; break;
    }
}

function animate() {
    requestAnimationFrame(animate);
    chaser.position.lerp(player.position, 0.02); // Chaser follows player
    renderer.render(scene, camera);
}
