# OS
Complete basic operating systems concepts with programmes


# 1st FCFS

```c

#include <stdio.h>

struct process {
    int pid, bt, wt, tt;
} p[10];

int main() {
    int n, awt = 0, att = 0, twt = 0, ttt = 0;

    printf("Enter the number of processes: ");
    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        printf("Enter the burst time for process %d: ", i);
        scanf("%d", &p[i].bt);
        p[i].pid = i;
    }

    p[0].wt = 0;
    p[0].tt = p[0].bt;

    for (int i = 1; i < n; i++) {
        p[i].wt = p[i - 1].wt + p[i - 1].bt;
        p[i].tt = p[i].wt + p[i].bt;
    }

    printf("\nPID \tBT \tWT \tTT\n");
    for (int i = 0; i < n; i++) {
        printf("%d \t%d \t%d \t%d\n", p[i].pid, p[i].bt, p[i].wt, p[i].tt);
        twt += p[i].wt;
        ttt += p[i].tt;
    }

    awt = twt / n;
    att = ttt / n;

    printf("Average Waiting Time = %d\n", awt);
    printf("Average Turnaround Time = %d\n", att);

    return 0;
}


```

# 2nd SJF 

```c
#include <stdio.h>

struct Process {
    int pid;
    int burst_time;
    int waiting_time;
    int turnaround_time;
};

void sjf_scheduling(struct Process p[], int n) {
    struct Process temp;

    // Sort processes based on burst time using Selection Sort
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if ((p[i].burst_time > p[j].burst_time) ||
                (p[i].burst_time == p[j].burst_time && p[i].pid > p[j].pid)) {
                temp = p[i];
                p[i] = p[j];
                p[j] = temp;
            }
        }
    }

    // Calculate waiting time and turnaround time
    p[0].waiting_time = 0;
    for (int i = 0; i < n; i++) {
        p[i].waiting_time = p[i - 1].waiting_time + p[i - 1].burst_time;
        p[i].turnaround_time = p[i].waiting_time + p[i].burst_time;
    }
}

int main() {
    int n;
    float avg_waiting_time = 0, avg_turnaround_time = 0;

    printf("Enter the number of processes: ");
    scanf("%d", &n);

    struct Process p[n];

    for (int i = 0; i < n; i++) {
        printf("Enter the burst time for process P%d (in ms): ", i + 1);
        scanf("%d", &p[i].burst_time);
        p[i].pid = i + 1;
    }

    sjf_scheduling(p, n);

    printf("\nSJF Scheduling\n\n");
    printf("Process  Burst Time  Turnaround Time  Waiting Time\n");
    for (int i = 0; i < n; i++) {
        printf("P%-8d %-11d %-16d %d\n", p[i].pid, p[i].burst_time, p[i].turnaround_time, p[i].waiting_time);
        avg_waiting_time += p[i].waiting_time;
        avg_turnaround_time += p[i].turnaround_time;
    }

    avg_waiting_time /= n;
    avg_turnaround_time /= n;

    printf("\nAverage Waiting Time: %.2f ms\n", avg_waiting_time);
    printf("Average Turnaround Time: %.2f ms\n", avg_turnaround_time);

    return 0;
}


```


# 3rd Round Robin


```c
#include <stdio.h>

struct process {
    int id, bt, wt, tt, rem;
} p[10];

int main() {
    int i, n, j, count = 0, quan, alot;
    float wtt = 0, ttt = 0;

    printf("Enter the number of processes: ");
    scanf("%d", &n);

    printf("Enter the burst time for processes:\n");
    for (i = 0; i < n; i++) {
        scanf("%d", &p[i].bt);
        p[i].id = i;
        p[i].wt = 0;
        p[i].rem = p[i].bt;
        p[i].tt = 0;
    }

    printf("Enter the quantum time: ");
    scanf("%d", &quan);

    i = 0;
    count = 0;

    printf("-------------------------------------------------------\n");

    while (count < n) {
        if (p[i].rem > 0) {
            alot = (p[i].rem >= quan) ? quan : p[i].rem;
            p[i].rem -= alot;
            p[i].tt += alot;

            for (j = 0; j < n; j++) {
                if (i != j && p[j].rem != 0) {
                    p[j].wt += alot;
                    p[j].tt += alot;
                }
            }

            if (p[i].rem == 0)
                count++;
        }

        i = (i + 1) % n;
    }

    for (i = 0; i < n; i++) {
        wtt += p[i].wt;
        ttt += p[i].tt;
    }

    wtt /= n;
    ttt /= n;

    printf("proc \t bt \t wt \t tt\n");
    for (i = 0; i < n; i++) {
        printf("%d\t%d\t%d\t%d\n", p[i].id, p[i].bt, p[i].wt, p[i].tt);
    }

    printf("Average wt=%f\n", wtt);
    printf("Average tt=%f\n", ttt);

    return 0;
}


```

# 4th First Fit


```c

#include<stdio.h>

struct segment {
    int id, size, allocation;
} seg[20];

int m, n;

void firstfit(int pn, int request) {
    int i, flag = 0;
    for (i = 0; i < m; i++) {
        if (request <= seg[i].size && seg[i].allocation == 0) {
            seg[i].allocation = 1;
            printf("\nProcess requirement %d is allocated to memory segment %d\n", request, seg[i].size);
            flag = 1;
            break;
        }
    }
    if (flag == 0)
        printf("Process requirement %d is not allocated\n", request);
}

void main() {
    int i, p[100];

    printf("Enter the number of memory segments: ");
    scanf("%d", &m);

    for (i = 0; i < m; i++) {
        printf("Enter the size of memory segment %d: ", i);
        scanf("%d", &seg[i].size);
        seg[i].id = i;
        seg[i].allocation = 0;
    }

    printf("Enter the number of processes: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++) {
        printf("\nEnter the request size of process p[%d]: ", i);
        scanf("%d", &p[i]);
        firstfit(i, p[i]);
    }
}


```

# 5th Best Fit


```c

#include <stdio.h>

struct segment {
    int id, size, allocation;
} seg[100];

int m, n, p[100];

void bestfit(int processNum, int requestSize) {
    int i, j, k, flag = 0, smallest, segmentId, temp[100];

    for (i = 0, k = 0; i < m; i++) {
        if (seg[i].size >= requestSize && seg[i].allocation == 0) {
            temp[k] = i;
            k++;
            flag = 1;
        }
    }

    if (flag == 0) {
        printf("Request memory %d is not allocated\n", requestSize);
        return;
    }

    smallest = seg[temp[0]].size;
    segmentId = temp[0];

    for (j = 1; j < k; j++) {
        if (seg[temp[j]].size < smallest) {
            smallest = seg[temp[j]].size;
            segmentId = temp[j];
        }
    }

    printf("Memory required %d is allocated to %d\n", requestSize, segmentId);
    seg[segmentId].allocation = 1;
}

int main() {
    int i;

    printf("Enter the number of memory segments: ");
    scanf("%d", &m);

    for (i = 0; i < m; i++) {
        printf("Enter the size of memory block %d: ", i);
        scanf("%d", &seg[i].size);
        seg[i].id = i;
        seg[i].allocation = 0;
    }

    printf("Enter the number of processes: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++) {
        printf("Enter the request size of process %d: ", i);
        scanf("%d", &p[i]);
        bestfit(i, p[i]);
    }

    return 0;
}


```

# 6th FIFO Page Replacement


```c

//Updated FIFO page replacement program

#include <stdio.h>

int main() {
    int referenceString[10], pageFaults = 0, currentPage, frameIndex, found, totalPages, totalFrames;

    printf("Enter the total number of pages: ");
    scanf("%d", &totalPages);

    printf("Enter the reference string values:\n");
    for (int i = 0; i < totalPages; i++) {
        printf("Page [%d]: ", i + 1);
        scanf("%d", &referenceString[i]);
    }

    printf("Enter the total number of frames: ");
    scanf("%d", &totalFrames);

    int frames[totalFrames];
    for (int i = 0; i < totalFrames; i++) {
        frames[i] = -1; // Initialize frames as empty (-1 represents an empty frame)
    }

    for (int i = 0; i < totalPages; i++) {
        currentPage = referenceString[i];
        found = 0;

        // Check if the current page is already present in a frame
        for (frameIndex = 0; frameIndex < totalFrames; frameIndex++) {
            if (frames[frameIndex] == currentPage) {
                found = 1;
                break;
            }
        }

        // If the current page is not found in any frame, perform page replacement
        if (found == 0) {
            int replaceIndex = -1, farthestPage = i;

            // Find the page in a frame that will be referenced farthest in the future
            for (frameIndex = 0; frameIndex < totalFrames; frameIndex++) {
                int nextOccurrence = -1;
                for (int j = i + 1; j < totalPages; j++) {
                    if (frames[frameIndex] == referenceString[j]) {
                        nextOccurrence = j;
                        break;
                    }
                }

                // If the page is not referenced anymore, replace it
                if (nextOccurrence == -1) {
                    replaceIndex = frameIndex;
                    break;
                }

                // If the page will be referenced farthest, update the farthestPage and replaceIndex
                if (nextOccurrence > farthestPage) {
                    farthestPage = nextOccurrence;
                    replaceIndex = frameIndex;
                }
            }

            // Replace the page in the frame with the current page
            frames[replaceIndex] = currentPage;
            pageFaults++;
        }

        // Print the current state of the frames
        printf("\n");
        for (frameIndex = 0; frameIndex < totalFrames; frameIndex++) {
            printf("%d\t", frames[frameIndex]);
        }
    }

    printf("\nTotal Page Faults: %d\n", pageFaults);
    return 0;
}

```


# 7th Optimal Page Replacement


```c

//Optimized Page Replacement algorithm...

#include <stdio.h>

#define MAX_FRAMES 10
#define MAX_PAGES 30

int main() {
    int no_of_frames, no_of_pages, frames[MAX_FRAMES], pages[MAX_PAGES], i, j, faults = 0;
    
    printf("Enter number of frames: ");
    scanf("%d", &no_of_frames);
    
    printf("Enter number of pages: ");
    scanf("%d", &no_of_pages);
    
    printf("Enter page reference string: ");
    for (i = 0; i < no_of_pages; ++i) {
        scanf("%d", &pages[i]);
    }
    
    // Initialize frames to -1, indicating empty frames
    for (i = 0; i < no_of_frames; ++i) {
        frames[i] = -1;
    }
    
    // Use a hash map to track page presence in frames
    int page_present[MAX_FRAMES] = {0};
    
    // Perform page replacement for each page in the reference string
    for (i = 0; i < no_of_pages; ++i) {
        int page = pages[i];
        
        // Check if the page is already present in one of the frames
        int page_found = page_present[page];
        
        // If the page is not found in any frame, perform page replacement
        if (!page_found) {
            int empty_frame = -1;
            
            // Find an empty frame to place the page
            for (j = 0; j < no_of_frames; ++j) {
                if (frames[j] == -1) {
                    empty_frame = j;
                    break;
                }
            }
            
            // If an empty frame is available, use it for the page
            if (empty_frame != -1) {
                frames[empty_frame] = page;
                page_present[page] = 1;
            } else {
                int max_pos = -1, pos = -1;
                
                // Find the page with the maximum position for replacement
                for (j = 0; j < no_of_frames; ++j) {
                    if (max_pos == -1 || page_present[frames[j]] == 0) {
                        pos = j;
                        max_pos = page_present[frames[j]];
                    }
                }
                
                // Replace the page in the selected frame
                page_present[frames[pos]] = 0;
                frames[pos] = page;
                page_present[page] = 1;
            }
            
            faults++; // Increment page fault count
        }
        
        // Print the contents of the frames after each page replacement
        printf("\n");
        for (j = 0; j < no_of_frames; ++j) {
            printf("%d\t", frames[j]);
        }
    }
    
    // Print the total number of page faults
    printf("\n\nTotal Page Faults = %d", faults);
    return 0;
}

```



# 8th Disk Scheduling

```c


#include <stdio.h>
#include <stdlib.h>

int main() {
    int queue[20], n, head, i, mov = 0, diff;
    
    printf("Enter the max range of disk: ");
    int max;
    scanf("%d", &max);
    
    printf("Enter the size of the queue request: ");
    scanf("%d", &n);
    
    printf("Enter the queue:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &queue[i]);
    
    printf("Enter the initial head position: ");
    scanf("%d", &head);
    queue[0] = head;
    
    printf("FROM\tTO\tHEAD MOVEMENTS\n");
    for (i = 1; i < n; i++) {
        diff = abs(queue[i] - queue[i-1]);
        mov += diff;
        printf("%d\t%d\t%d\n", queue[i], queue[i + 1], mov);
    }
    
    printf("Total head movements are %d\n", mov);

    return 0;
}

```

